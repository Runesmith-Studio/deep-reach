<div align="center">

# 🛰️ deep-reach

### Ask once. Get a cited, gap-honest research brief.

**AI social listening · market research · competitive intelligence — as one Claude Code skill.**

[![License: MIT](https://img.shields.io/badge/License-MIT-black.svg)](LICENSE)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-skill-da7756.svg)](https://docs.claude.com/en/docs/claude-code)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#contributing)

[中文说明 →](README.zh-CN.md)

</div>

---

> **No more $500/month social-listening dashboards you barely open.**
> **No more juggling a trend scanner in one tab and a scraper in another.**
> **No more copy-pasting Reddit threads, YouTube transcripts, and forum posts into a doc by hand.**
>
> Just ask. **deep-reach** scans what people said in the last 30 days, reads the
> *real* originals across 13+ platforms, and hands you a **cited brief that tells
> you how much to trust it** — in one skill, inside Claude Code.

---

## What you get

You ask a question in plain language — *"what do people really think about X?"*,
*"competitor research on Y, in users' own words"*, *"is this niche worth it?"* —
and deep-reach runs the whole research loop for you:

```
①  PULSE   →   ②  DIG   →   ③  BRIEF
```

1. **Pulse** — finds *where* your topic is hot right now and which way the
   conversation is moving.
2. **Dig** — reads the **full originals** (posts, comment threads, video
   transcripts, articles) across **Reddit, X, YouTube, GitHub, Bilibili,
   XiaoHongShu, V2EX, and the open web**.
3. **Brief** — writes a **cited evidence brief** with quoted sources, declared
   blind spots, and a **trust rating** (thick / medium / thin) so you never
   mistake a thin hunch for a hard finding.

→ See a [sample brief](examples/example-brief.md).

---

## Why deep-reach

|  | Enterprise listening suites | DIY tool-juggling | **deep-reach** |
|---|---|---|---|
| Price | $500–5,000 / mo | "free" (your hours) | **free, open source** |
| Setup | Sales call + onboarding | Glue 5 tools yourself | **one skill, drop-in** |
| Reads full originals | partial | manual | **✅ automatic** |
| Chinese platforms (Bilibili/Xiaohongshu/Xueqiu) | rarely | hard | **✅ built in** |
| Tells you how much to trust it | ❌ | ❌ | **✅ trust rating + blind spots** |
| Real user quotes, cited | dashboards | copy-paste | **✅ quoted + linked** |
| Lives where you work | separate app | many apps | **✅ inside Claude Code** |

**The pitch in one line:** the reach of an enterprise intelligence suite and the
freshness of a live trend scan — minus the price tag, the tab-juggling, and the
guesswork about whether the answer is solid.

---

## Features

- 🔭 **Social listening & buzz tracking** — last-30-day pulse on any topic, brand, or event.
- 🧭 **Market & competitor research** — surface real complaints, praise, and demand signals in customers' own words.
- ✅ **Product / niche validation** — gather honest demand evidence *before* you build.
- 🗣️ **Voice-of-customer mining** — the exact phrases users use, ready to reverse into copy or keywords.
- 🌏 **13+ platforms, English & Chinese** — Reddit, X, YouTube, GitHub, Bilibili, XiaoHongShu, V2EX, web, and more.
- 🧪 **Evidence discipline built in** — every brief declares its coverage, its blind spots, and a trust rating. No dressed-up guesses.
- 🔒 **Private & free-first** — zero-account by default; never touches your main accounts or your daily-browser cookies.

---

## Install

```bash
# 1. Add the skill to Claude Code
git clone https://github.com/Runesmith-Studio/deep-reach.git
cp -r deep-reach/skills/deep-reach ~/.claude/skills/
```

That's it — start asking. For the **deepest** multi-platform reach, deep-reach can
also drive two optional research engines if present (it sets them up for you):

```bash
# optional power-ups (recommended for full reach)
pipx install https://github.com/Panniantong/agent-reach/archive/main.zip
agent-reach install --env=auto      # free multi-platform channels
pipx install bilibili-cli           # login-free Bilibili search
#  + the last30days skill: https://github.com/mvanhorn/last30days-skill
# optional Hermes Agent X/Twitter route:
#   hermes plugins install Xquik-dev/hermes-tweet --enable
#   export XQUIK_API_KEY="your-xquik-api-key"
```

Without them, deep-reach still works wherever your Claude host exposes web
search/fetch — just with shallower reach. It will always tell you what it
couldn't cover rather than pretend.

When running in Hermes Agent, [Hermes Tweet](https://github.com/Xquik-dev/hermes-tweet) can supply an optional read-only X/Twitter route for Phase 2. Keep `HERMES_TWEET_ENABLE_ACTIONS` unset; deep-reach needs source collection, not X/Twitter write actions.

---

## Use

Just ask Claude things like:

- *"research what people actually say about \<topic\>, dig into the originals"*
- *"competitor research on \<product\> — real user complaints, cited"*
- *"is \<niche\> worth building? show me real demand signals"*

deep-reach runs Pulse → Dig → Brief and writes the result to `./research/`.

---

## Contributing

Issues and PRs welcome. If deep-reach saved you a research scramble, a ⭐ helps
others find it.

## Powered by

deep-reach is an orchestration skill. Full credit to the open tools it can build on:
[last30days](https://github.com/mvanhorn/last30days-skill) and
[agent-reach](https://github.com/Panniantong/Agent-Reach).

## License

MIT © [Runesmith Studio](https://github.com/Runesmith-Studio) — an independent AI app studio.

<sub>Keywords: AI market research · social listening · competitive intelligence · competitor analysis · consumer insights · voice of customer · OSINT · Reddit / X / YouTube / Bilibili / Xiaohongshu research · Claude Code skill · agent skill</sub>
