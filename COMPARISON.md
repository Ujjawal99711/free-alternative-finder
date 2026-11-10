# Free Alternative Finder vs. Normal LLM Web Search

**A side-by-side test on one real question**
*Run date: 10 November 2026 · Question: "I don't want to pay for video editing apps"*

---

## TL;DR

| | Winner |
|---|---|
| Finding options (especially new ones) | 🏆 **Skill**: 15 tools vs 6, including 6 the web search never mentioned |
| Accuracy of facts (stars, dates, licenses) | 🏆 **Skill**: exact, live and per repo, vs ranges and gaps |
| Catching risks | 🏆 **Skill**: 5+ specific flags vs 1 |
| Explaining what's pleasant to use | 🏆 **Web search**: better qualitative context |
| Speed and readability | 🏆 **Web search**: shorter and faster |

**Bottom line:** web search summarizes what people *wrote* about GitHub. The skill checks what is *actually on GitHub*. For finding and vetting free alternatives, the skill is clearly better. For a beginner-friendly "which one should I pick", web search reads better, so the skill was updated to borrow that strength.

---

## 1. Test setup

Both runs used Claude on the same day with the same plain-English request:

> *I don't want to pay for video editing apps*

| | Run A: Normal web search | Run B: Free Alternative Finder skill |
|---|---|---|
| Tool | Claude with web search on | Claude with this skill |
| Where data came from | Search engine results: blogs, directories, news posts | GitHub's search API + each repo's own README |
| Instructions | None beyond the question | The skill's 6-step method |

**Caveat:** this is one question and one sample of each. Web search results vary between runs. The comparison shows the *structural* differences between the two approaches, not a benchmark.

---

## 2. How each approach works

### Run A: Normal LLM web search

```
Question → search engine → read top articles (listicles, directories, news)
         → summarize what the articles say → answer
```

- **Source type:** secondhand. Someone else already wrote about these tools.
- **Freshness:** only as fresh as the articles. A tool that launched last month is invisible until a blogger covers it.
- **Numbers:** copied from whichever article mentioned them, so they're often stale and inconsistent between sources.

### Run B: Free Alternative Finder

```
Question → restate need + constraints
         → 8 different GitHub queries (topics, keywords, "X alternative", synonyms)
         → merge, dedupe, drop forks / wrappers / libraries
         → read each top repo's README to confirm it really does the job
         → rank by stars, add momentum + freshness, flag risks
         → table + best fit + skipped list + timestamp + queries used
```

- **Source type:** firsthand. GitHub's own data, plus the project's own README.
- **Freshness:** live, as of the minute it runs.
- **Numbers:** exact, from the same source, applied the same way to every repo.

---

## 3. Results side by side

### 3.1 Coverage

| | Web search | Skill |
|---|---|---|
| Full video editors listed | 4 (OpenCut, Shotcut, Kdenlive, OpenShot) | 12 |
| Helper tools listed | 1 (LosslessCut) | 3 (LosslessCut, auto-editor, AI Shorts Generator) |
| "Avoid" recommendations | 1 (Olive) | 1 (Olive), plus caution flags on 4 others |
| Explicitly skipped, with reasons | None | Libraries, AI research models, agent tools, Android SDKs |

**Tools only the skill found:**

| Tool | Why it matters |
|---|---|
| [jub0t/Concat](https://github.com/jub0t/Concat) | Arguably the best fit for short-form creators: auto-captions, text-to-speech, background removal, fully offline |
| [Augani/openreel-video](https://github.com/Augani/openreel-video) | Browser-based CapCut alternative, portrait export, no watermark |
| [walterlow/freecut](https://github.com/walterlow/freecut) | Browser editor, nothing to install, files stay local |
| [CutWire-Studios/Drift](https://github.com/CutWire-Studios/Drift) | Beginner editor aimed at Reels and Shorts |
| [AIEraDev/Clypra](https://github.com/AIEraDev/Clypra) | Hardware-accelerated cross-platform editor |
| [storytold/filmcraft](https://github.com/storytold/filmcraft) | Pro-style editor, flagged for an unusual star pattern |

Most of these are only months old, which is exactly the window blog-based search can't see yet.

### 3.2 Accuracy of facts

| Fact | Web search said | Skill said (live from GitHub) |
|---|---|---|
| OpenCut stars | "about 85k–97k" | 98,017 |
| LosslessCut stars | "about 43k" | 45,893 |
| Olive last activity | "a year ago" | 5 Dec 2024, so about 23 months |
| OpenCut status | Mentions the rewrite but still says "start here" for reels | Mid-rewrite, so it points to `opencut-classic` as the usable version today |
| Licenses | Not given | Given for every repo, including OpenShot's license not being machine-detectable |
| Platforms | Given for some | Given for every repo, e.g. Palmier Pro is macOS 26 + Apple Silicon only |

The web search answer even admitted its star counts were approximate because different trackers disagreed. The skill avoids that problem because it only uses one source of truth.

### 3.3 Risk detection

| Risk | Web search | Skill |
|---|---|---|
| Stalled project (Olive) | ✅ Caught (date slightly off) | ✅ Caught, with exact date |
| Suspicious popularity (FilmCraft: 18k stars in 6 weeks, 41% fork ratio) | ❌ | ✅ Flagged |
| License change (Palmier Pro moved off GPL after v0.7.6) | ❌ | ✅ Flagged |
| Beta software (Concat) | ❌ | ✅ Flagged |
| Platform lock-in (Palmier Pro: macOS 26 only) | ❌ | ✅ Flagged |
| Paid API hidden inside a "free" tool (Shorts Generator default mode) | ❌ | ✅ Flagged, with how to keep it free |
| Telemetry (Clypra) | ❌ | ✅ Noted |

### 3.4 Where web search did better

To be fair, the web search answer had real strengths:

- **Human context:** "easiest one to learn", "often called the open-source Premiere Pro", and simple picks by use case (reels / YouTube / beginners). The skill described what tools *do* better than how they *feel*.
- **Release news:** it surfaced recent version highlights (e.g. hardware decoding in a new Shotcut release), which the skill doesn't look for.
- **Brevity:** about 6 recommendations in a few hundred words. Easier for a beginner to act on.
- **Speed:** seconds instead of a few minutes.

### 3.5 Where both had blind spots

- **Mature projects that live off GitHub** (Kdenlive's main home is KDE's GitLab) look smaller than they are. Web search handled this through articles; the skill noticed and said so explicitly.
- **Neither can judge real quality** (export reliability, smoothness) without hands-on testing or user reviews.

---

## 4. Scorecard

Scores out of 5, based on this test.

| Criterion | Web search | Skill |
|---|:---:|:---:|
| Discovery (finds the full landscape, including new tools) | 2 | **5** |
| Factual accuracy (stars, dates, licenses) | 2 | **5** |
| Per-result detail (license, platform, freshness) | 2 | **5** |
| Risk flagging | 2 | **5** |
| Every link works / no made-up repos | 4 | **5** |
| Reproducibility (timestamp + exact queries) | 1 | **5** |
| Qualitative "is it nice to use" context | **5** | 2 |
| Brevity and beginner-friendliness | **5** | 3 |
| Speed | **5** | 3 |
| **Total** | **28** | **38** |

---

## 5. Why the difference exists

| | Web search | Skill |
|---|---|---|
| What it reads | What people **wrote about** GitHub | **GitHub itself** |
| Freshness | As old as the newest article | Live |
| Consistency | Whatever each article chose to mention | Same checks applied to every repo |
| Incentives in the source | SEO listicles, affiliate links, vendor blogs | None: raw repo data |
| Output | A summary | A verifiable, reproducible table |

The core difference is **secondhand vs firsthand**. Secondhand sources are good at opinions and bad at facts and new arrivals. Firsthand data is the reverse.

---

## 6. What we changed after this test

The comparison showed the skill was weak exactly where web search was strong. So the skill now:

1. Runs a **quick web search for each of its top 1–3 picks** and adds a short "what real users say" note, clearly marked as coming from the web.
2. **Never lets the web override GitHub facts.** Stars, licenses and dates still come only from GitHub.
3. Falls back to **raw README files** when the GitHub README API is blocked (this happened during the test).
4. **Checks its own setup first** (tools, network, optional token), so it fails clearly instead of halfway.

---

## 7. When to use which

| Your question | Use |
|---|---|
| "What free tools exist for X?" | **Skill** |
| "Is there anything new I'm missing?" | **Skill** |
| "Can I use this commercially / on my laptop without a GPU?" | **Skill** |
| "Track this category for me every day" | **Skill** (daily mode) |
| "Just tell me the easiest one for a beginner" | Web search, or the skill's "Best fit" section |
| "What changed in the latest version of X?" | Web search |

---

*Both runs: 10 November 2026, around 21:30 IST. Skill queries: `topic:video-editor`, `topic:video-editing`, `video editor in:name,description`, `non-linear video editor`, `premiere pro alternative`, `capcut alternative`, `open source video editor`, `adobe premiere alternative`, each with `stars:>100 archived:false`. Full skill output: [examples/video-editors-2026-11-10.md](examples/video-editors-2026-11-10.md).*
