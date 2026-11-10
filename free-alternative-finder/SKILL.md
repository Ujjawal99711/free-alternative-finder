---
name: "free-alternative-finder"
description: "Use when the user describes a problem or a paid tool they want to replace and wants free, open-source GitHub alternatives ranked by stars with live links."
---

# Free Alternative Finder

Turn a problem statement ("I want ElevenLabs-quality voices for free", "I need a self-hosted Notion") into a verified, star-ranked list of open-source GitHub repos the user can actually run, each with its GitHub URL.

Core rule: every repo, star count, license and date in the answer must come from the GitHub API in THIS run. Never list a repo or a number from memory or from a blog post.

## Step 0: Preflight (run once per session, takes seconds)

Check the environment before searching, so the run never fails halfway:

```bash
command -v curl >/dev/null && command -v python3 >/dev/null && echo tools-ok
curl -s -o /dev/null -w "api:%{http_code}\n" "https://api.github.com/search/repositories?q=test&per_page=1"
curl -s -o /dev/null -w "raw:%{http_code}\n" https://raw.githubusercontent.com/github/gitignore/HEAD/README.md
[ -n "$GITHUB_TOKEN" ] && echo token-found || (gh auth token >/dev/null 2>&1 && echo gh-token-found)
```

Then act on the results:
- Tools missing: install them yourself (`apt-get install -y curl` or use the available package manager). Do not ask the user.
- `api` or `raw` returns 403 with "host not allowed", or fails to connect: this is a network setting only the user can change. Stop and tell them exactly what to do:
  - claude.ai: Settings > Capabilities, turn on "Code execution and file creation" and "Allow network egress". On Team/Enterprise, an admin may need to add `api.github.com` and `raw.githubusercontent.com` to the allowed domains.
  - Claude Code: approve the network request when prompted, or add both domains to `sandbox.network.allowedDomains`.
- `raw` works but `api` doesn't: continue, but fall back to web search to find candidate repos, and still verify each one through its raw README and GitHub page.
- Token found: use it (higher rate limits). If no token, continue anonymously. Never ask the user to paste a token into the chat. If they want higher limits, tell them to set `GITHUB_TOKEN` in their environment.

## Step 1: Pin down the problem

Restate the need in one line: the job to be done, plus the paid product it replaces, if any.
Note constraints from the conversation: OS, GPU or CPU-only, commercial use needed, language, self-hosted vs desktop app.
If a constraint would change the ranking a lot (e.g. commercial use, no GPU) and is unknown, ask ONE short question. Otherwise, state your assumption and continue.

## Step 2: Search GitHub from several angles

Write 4-8 different queries, not rewordings. Mix:
- topics: `topic:text-to-speech`, `topic:voice-cloning`
- plain keywords: `voice cloning tts`
- replacement phrasing: `elevenlabs alternative`, `open source elevenlabs`, `self-hosted notion`
- synonyms of the job: `speech synthesis`, `zero-shot tts`

Run each one sorted by stars, excluding archived repos and noise. Use curl (it works everywhere, with or without `gh`):

```bash
Q="QUERY stars:>100 archived:false"
curl -s ${GITHUB_TOKEN:+-H "Authorization: Bearer $GITHUB_TOKEN"} \
  "https://api.github.com/search/repositories?q=$(python3 -c 'import urllib.parse,sys;print(urllib.parse.quote(sys.argv[1]))' "$Q")&sort=stars&per_page=30" \
| python3 -c 'import sys,json
for i in json.load(sys.stdin).get("items",[]):
  print("\t".join(map(str,[i["full_name"],i["stargazers_count"],i["forks_count"],(i.get("license") or {}).get("spdx_id","none"),i["pushed_at"],i["created_at"],i["html_url"],(i["description"] or "").replace("\t"," "),i["fork"]])))' >> raw.tsv
sleep 7
```

The `sleep 7` keeps you under GitHub's anonymous limit of about 10 searches a minute. If a response says "rate limit exceeded", wait 60 seconds and retry once.

## Step 3: Merge and filter

- Dedupe by full_name.
- Drop forks.
- Put awesome-lists and curated lists in a separate "lists worth browsing" note, not in the ranking.
- Drop SDKs or clients for the paid product itself (e.g. an ElevenLabs API wrapper is not a free alternative).
- Mark, but keep, repos with no push in more than 12 months as "possibly unmaintained".

## Step 4: Verify the top candidates

For the top ~15 by stars, read the README from raw GitHub (the README API is often blocked in sandboxes, raw files usually are not):

```bash
for f in README.md readme.md README.rst README; do
  curl -sf "https://raw.githubusercontent.com/OWNER/REPO/HEAD/$f" && break
done | grep -v -E '^\s*$|<img|shields.io|badge' | head -60
```

Confirm it really does the job, then extract:
- runs locally? (desktop app / Docker / pip / web UI)
- hardware (CPU OK? GPU and VRAM needed?)
- license, and whether commercial use is allowed (non-commercial, custom or missing license gets a flag)
- a one-line plain-English description of what it does

Drop anything that turns out to be off-topic.

## Step 5: Rank and sanity-check

Rank primarily by stars, as the user asked. Also show momentum and freshness so stars don't mislead:
- momentum = stars per month since created_at
- last update = pushed_at

Add a warning flag when:
- stars look inflated: very high stars with forks under ~2% of stars, or a very young repo with huge stars
- the license is missing or non-commercial
- there has been no update in more than 12 months
- it needs a GPU the user said they don't have

## Step 6: Answer

1. One line restating the problem and any assumptions.
2. A ranked table with these columns: #, Repo (as a link `[owner/repo](https://github.com/owner/repo)`), Stars, Last update, License, Runs on, What it does, Flags.
3. "Best fit for you": 1-3 picks matched to the user's constraints, one sentence each on why. This may differ from the #1 by stars, so say so.
   Before writing this section, if web search is available, run one quick search per pick (e.g. `<repo name> review` or `<repo name> reddit`) and add a short note on what real users report (easy to learn, crashes on export, great for 4K). Mark these notes as coming from the web, not from GitHub. Never use the web to change a star count, license or date.
4. Skipped: a short list of notable repos excluded and why (wrapper, archived, off-topic).
5. Footer: `Live GitHub data as of <date/time>` and the queries used.

Keep the prose short. The table is the product.

## Daily mode

When run on a schedule for a saved problem:
- compare against the previous run's list, if one is available
- report only what changed: new entrants, big star jumps, repos newly archived or with a changed license
- if nothing changed, say so in one line