# Free Alternative Finder

**Stop paying for software that has a free, open-source twin on GitHub.**

Free Alternative Finder is a skill for [Claude](https://claude.ai) and [Claude Code](https://claude.com/claude-code). You describe a problem in plain English, like *"I don't want to pay for video editing apps"* or *"I want ElevenLabs-quality voices for free"*. It searches GitHub live, checks every candidate, and gives you back a ranked table of open-source tools you can actually run. Each entry has its GitHub link, real star count, license, platforms and warning flags.

> Normal AI web search tells you what the internet *wrote* about alternatives.
> This skill checks what's *actually on GitHub right now*, and whether you can use it.

📊 **[Read the full comparison report: skill vs. normal LLM web search →](COMPARISON.md)**

---

## Contents

- [Why this exists](#why-this-exists)
- [What you get](#what-you-get)
- [How it works](#how-it-works)
- [Install](#install)
- [Requirements](#requirements)
- [Usage](#usage)
- [Daily mode](#daily-mode)
- [How it compares to normal web search](#how-it-compares-to-normal-web-search)
- [Limitations](#limitations)
- [Repo structure](#repo-structure)
- [Contributing](#contributing)
- [License](#license)

---

## Why this exists

Searching Google or an AI chatbot for "free alternative to X" gets you:

- **SEO listicles** that are months old, often written by a competitor of the tool you're replacing
- **Star counts that are guessed or copied** from old articles ("about 85k–97k stars")
- **Nothing new.** A tool launched six weeks ago hasn't been written about yet, so search can't find it, and in AI that's often the best option
- **No answer to the questions that matter:** Does it run on my laptop? Do I need a GPU? Can I use it commercially? Is it still maintained?

This skill goes straight to the source, GitHub itself, and answers those questions for every result.

## What you get

A real run from 10 Nov 2026 for *"I don't want to pay for video editing apps"* (abridged; [full output here](examples/video-editors-2026-11-10.md)):

| # | Repo | Stars | Last update | License | Runs on | Flags |
|---|---|---|---|---|---|---|
| 1 | [OpenCut-app/OpenCut](https://github.com/OpenCut-app/OpenCut) | 98,017 | 24 Sep 2026 | MIT | Web, desktop, mobile | Mid-rewrite: use opencut-classic today |
| 2 | [storytold/filmcraft](https://github.com/storytold/filmcraft) | 17,980 | 10 Nov 2026 | Apache-2.0 | Mac, Win, Linux, web | ⚠️ 6 weeks old, unusual star/fork pattern |
| 3 | [jub0t/Concat](https://github.com/jub0t/Concat) | 16,478 | 10 Nov 2026 | AGPL-3.0 | Mac, Win, Linux, Android | ⚠️ Beta |
| 4 | [mltframework/shotcut](https://github.com/mltframework/shotcut) | 15,647 | 4 Nov 2026 | GPL-3.0 | Mac, Win, Linux | None |
| … | … | … | … | … | … | … |

Every answer includes:

1. **A ranked table:** repo link, stars, last update, license, platforms, one-line description and warning flags
2. **Best fit for you:** 1–3 picks matched to *your* constraints (OS, GPU, commercial use), with notes on what real users say
3. **Skipped:** what was left out and why (API wrappers, archived repos, off-topic libraries)
4. **Proof:** a timestamp and the exact queries used, so anyone can reproduce the result

## How it works

```
Your problem ──► 0. Preflight      checks tools + GitHub access, fixes what it can
                 1. Pin down       restates the need, notes OS / GPU / license constraints
                 2. Search         4–8 different GitHub queries, sorted by stars
                 3. Filter         dedupe, drop forks, wrappers, awesome-lists
                 4. Verify         reads each top repo's README: does it really do the job?
                 5. Rank + check   stars, momentum, freshness, red flags
                 6. Answer         table + best fit + real-user notes + sources
```

**The core rule:** every repo, star count, license and date in the answer must come from GitHub in *that* run. Nothing comes from memory or from blog posts. So you never get a made-up repo or a broken link.

**Red flags it catches automatically:**

- ⭐ **Suspicious stars:** huge star counts on very young repos, or odd star-to-fork ratios
- 📜 **License problems:** missing, non-commercial or recently changed licenses
- 💤 **Abandoned projects:** no update in more than 12 months
- 🖥️ **Hardware mismatch:** needs a GPU you said you don't have
- 🔌 **Fake alternatives:** wrappers that still call the paid product's API

## Install

### Claude (claude.ai, desktop, mobile)

1. Download [`free-alternative-finder.zip`](free-alternative-finder.zip) from this repo.
2. In Claude, open **Settings → Capabilities** and turn on **Code execution and file creation**.
3. In the **Skills** section, upload the ZIP and make sure the skill is toggled on.

### Claude Code

```bash
git clone https://github.com/Ujjawal99711/free-alternative-finder.git
mkdir -p ~/.claude/skills
cp -r free-alternative-finder/free-alternative-finder ~/.claude/skills/
```

Restart Claude Code. The skill is available as `/free-alternative-finder`, and Claude also picks it up automatically when you ask for free alternatives.

## Requirements

| Requirement | Needed? | Notes |
|---|---|---|
| Claude Pro, Max, Team or Enterprise | ✅ Yes (on claude.ai) | Skills need a paid plan on claude.ai |
| Code execution turned on | ✅ Yes | Settings → Capabilities |
| Network access for code | ✅ Yes | Turn on **Allow network egress**. Needs `api.github.com` and `raw.githubusercontent.com`. Team/Enterprise admins may need to allow these |
| `curl` + `python3` | ✅ Yes | Present in almost every environment. The skill installs them if missing |
| GitHub account | ❌ No | Everything it reads is public |
| GitHub connector / `gh` CLI | ❌ No | Uses plain `curl` |
| GitHub token | ⚪ Optional | Set `GITHUB_TOKEN` in your environment for higher rate limits. **Never paste a token into chat** |
| Web search | ⚪ Optional | Adds "what real users say" notes to the top picks |

The skill checks all of this itself before searching (Step 0). If something only you can fix is blocking it, like a network setting, it tells you exactly which switch to flip.

## Usage

Just describe the problem. Constraints help but aren't required:

```
/free-alternative-finder I don't want to pay for video editing apps
```

```
I want ElevenLabs-quality voiceovers for my reels, free, no GPU, commercial use OK
```

```
Is there a self-hosted Notion I can run on a cheap VPS?
```

```
Free alternative to Opus Clip for turning podcasts into shorts
```

## Daily mode

Pair the skill with a Claude scheduled task to track a category over time, e.g. *"Every morning, run free-alternative-finder for AI voice tools and tell me what changed."* In daily mode it reports only the differences: new entrants, big star jumps, repos that were archived or changed license. If nothing changed, it says so in one line.

## How it compares to normal web search

We ran the same question, *"I don't want to pay for video editing apps"*, through this skill and through normal Claude web search on the same day.

| | Normal LLM web search | This skill |
|---|---|---|
| Options found | 5 editors + 1 to skip | 12 editors + 3 helper tools |
| New tools discovered | None | 6 (Concat, OpenReel, FreeCut, Drift, FilmCraft, Clypra) |
| Star counts | Ranges, admitted to be approximate | Exact, live |
| License / platform / last update | Mostly missing | For every repo |
| Risk flags | 1 (stalled project, date slightly off) | 5+ (suspicious stars, license change, beta, Mac-only, stalled with exact date) |
| "Is it nice to use?" context | ✅ Strong | ⚪ Added via the real-user notes step |
| Sources | Blogs and directories | GitHub itself |

**Verdict:** for *discovering* and *fact-checking* options, the skill wins clearly. For "just tell me which one is pleasant to use", web search reads better, so the skill now borrows that strength by adding real-user notes to its top picks.

👉 **[Full report with methodology and analysis](COMPARISON.md)**

## Limitations

- **Stars measure popularity, not quality.** A hyped 6-week-old repo can outrank a proven 14-year-old one. That's why the skill shows momentum and freshness and flags anomalies.
- **Some great projects live mostly off GitHub** (e.g. Kdenlive on KDE's GitLab), so their GitHub stars undercount them. The skill notes this when it spots it.
- **It can't judge feel.** Voice quality, editing smoothness and UX still need a quick hands-on test or real-user reviews.
- **Rate limits.** Without a token, GitHub allows about 10 searches a minute. One run fits comfortably, but back-to-back runs may pause briefly.
- **A run takes a few minutes**, because it reads READMEs instead of skimming snippets.

## Repo structure

```
.
├── free-alternative-finder/
│   └── SKILL.md                      # the skill itself
├── free-alternative-finder.zip       # ready-to-upload package for claude.ai
├── examples/
│   └── video-editors-2026-11-10.md   # full real output
├── COMPARISON.md                     # skill vs. normal LLM web search report
├── LICENSE
└── README.md
```

## Contributing

Ideas and PRs welcome, especially:

- better red-flag heuristics (fake-star detection, license parsing)
- more example runs across categories (TTS, note-taking, design, automation)
- category-specific query packs

Open an issue describing the problem you tried, what the skill returned, and what you expected.

## License

[MIT](LICENSE). Use it, fork it, share it.

---

Built by [Ujjawal](https://github.com/Ujjawal99711) with Claude.
