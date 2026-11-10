# Example run: "I don't want to pay for video editing apps"

*Live GitHub data as of 10 Nov 2026, 21:30 IST.*

**Problem:** free, open-source replacement for paid video editors (Premiere Pro, CapCut and similar).
**Assumptions:** desktop editing for reels, Shorts and YouTube; normal laptop; free for personal or creator use is enough.

## Full video editors

| # | Repo | Stars | Last update | License | Runs on | What it does | Flags |
|---|---|---|---|---|---|---|---|
| 1 | [OpenCut-app/OpenCut](https://github.com/OpenCut-app/OpenCut) | 98,017 | 24 Sep 2026 | MIT | Web, desktop, mobile | "The open-source CapCut alternative" | Being rewritten from scratch. The usable version today is [opencut-classic](https://github.com/opencut-app/opencut-classic) |
| 2 | [storytold/filmcraft](https://github.com/storytold/filmcraft) | 17,980 | 10 Nov 2026 | Apache-2.0 | Mac, Windows, Linux, browser | Premiere-style pro editor with color grading, written in Rust | ⚠️ Only 6 weeks old with 18k stars, and a 41% fork ratio. Unusual |
| 3 | [jub0t/Concat](https://github.com/jub0t/Concat) | 16,478 | 10 Nov 2026 | AGPL-3.0 | Mac, Windows, Linux, Android | CapCut-style: auto-captions, text-to-speech, background removal, 100 effects, fully offline | ⚠️ Beta, and only 2.5 months old |
| 4 | [mltframework/shotcut](https://github.com/mltframework/shotcut) | 15,647 | 4 Nov 2026 | GPL-3.0 | Mac, Windows, Linux | Mature, stable all-round editor | None |
| 5 | [palmier-io/palmier-pro](https://github.com/palmier-io/palmier-pro) | 14,724 | 2 Nov 2026 | GPL-3.0* | macOS 26, Apple Silicon only | Premiere-like editor with built-in AI generation | ⚠️ Newer releases are no longer GPL. AI generation likely needs paid models |
| 6 | [olive-editor/olive](https://github.com/olive-editor/olive) | 9,152 | 5 Dec 2024 | GPL-3.0 | Mac, Windows, Linux | Non-linear editor | ⚠️ No update in about 2 years, and alpha/unstable |
| 7 | [OpenShot/openshot-qt](https://github.com/OpenShot/openshot-qt) | 6,763 | 10 Nov 2026 | Not detected | Mac, Windows, Linux | Beginner-friendly editor with titles, keyframes and transitions | GitHub couldn't read the license, so check it |
| 8 | [KDE/kdenlive](https://github.com/KDE/kdenlive) | 6,084 | 10 Nov 2026 | GPL-3.0 | Mac, Windows, Linux | Powerful pro-grade editor | None (GitHub is a mirror, so stars undercount it) |
| 9 | [Augani/openreel-video](https://github.com/Augani/openreel-video) | 5,631 | 17 Oct 2026 | MIT | Browser + desktop | CapCut-style: captions, audio ducking, color, portrait export | None |
| 10 | [AIEraDev/Clypra](https://github.com/AIEraDev/Clypra) | 3,355 | 9 Nov 2026 | MIT | Mac, Windows, Linux, iOS, Android | Fast, hardware-accelerated timeline editor | Sends anonymous performance telemetry |
| 11 | [CutWire-Studios/Drift](https://github.com/CutWire-Studios/Drift) | 2,378 | 10 Nov 2026 | GPL-3.0 | Mac, Windows, Linux, Android | Beginner editor for Reels and Shorts with captions, stickers and music | Young (4 months) |
| 12 | [walterlow/freecut](https://github.com/walterlow/freecut) | 2,321 | 29 Oct 2026 | MIT | Browser | Multi-track editor with nothing to install, files stay local | None |

## Helper tools (not full editors)

| Repo | Stars | What it does |
|---|---|---|
| [mifi/lossless-cut](https://github.com/mifi/lossless-cut) | 45,893 | Trims and joins huge clips in seconds without re-encoding |
| [Anil-matcha/AI-Youtube-Shorts-Generator](https://github.com/Anil-matcha/AI-Youtube-Shorts-Generator) | 5,892 | Opus Clip alternative that turns long videos into 9:16 shorts. The default mode uses a paid API, so use `--mode local` to keep it free |
| [WyattBlue/auto-editor](https://github.com/WyattBlue/auto-editor) | 5,835 | Command-line tool that cuts out silences automatically |

## Best fit

- **For reels with captions and voiceover: Concat.** The closest thing to free CapCut, and it works offline. It's in beta, so keep projects backed up.
- **For something that won't break: Kdenlive or Shotcut.** Both have been around for over 10 years. Their low star counts mislead, because Kdenlive is mainly developed on KDE's own GitLab.
- **If you don't want to install anything: OpenReel.** Runs in the browser and exports portrait video with no watermark.

None of these is #1 by stars. OpenCut is mid-rewrite, and FilmCraft's numbers need a second look before you trust it.

## Skipped

- Coding libraries, not apps: moviepy, editly, FFCreator
- AI generation research: LivePortrait, VACE, Text2Video-Zero
- Tools that write videos from code for AI agents: HyperFrames, ffmpeg-skill
- Off-topic: Android camera SDKs
- Awesome list worth browsing: [showlab/Awesome-Video-Diffusion](https://github.com/showlab/Awesome-Video-Diffusion) (AI video only)

## Queries used

`topic:video-editor`, `topic:video-editing`, `video editor in:name,description`, `non-linear video editor`, `premiere pro alternative`, `capcut alternative`, `open source video editor`, `adobe premiere alternative`, each with `stars:>100 archived:false`, sorted by stars.

*This run used the first version of the skill. The real-user notes step was added afterwards.*
