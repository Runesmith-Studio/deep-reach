# deep-reach

**Turn a 30-day community pulse into a cited, gap-honest research brief.**

`deep-reach` is a Claude Code skill that chains two great open tools into one
disciplined pipeline for **public-opinion & market research**:

- **[last30days](https://github.com/mvanhorn/last30days-skill)** — the *thermometer*: finds **where** a topic is being discussed and how hot it is, across Reddit, X, YouTube, TikTok, Hacker News, GitHub and more.
- **[agent-reach](https://github.com/Panniantong/Agent-Reach)** — the *reach*: reads the **full originals** — posts, comment bodies, video transcripts, web pages — across 13+ platforms including Chinese ones (Bilibili, XiaoHongShu, V2EX, Xueqiu).

Neither tool alone gives you a *finished* answer. last30days is fast but thin and
prone to single-source concentration. agent-reach reads deeply but doesn't score
30-day trends or enforce evidence hygiene. **deep-reach is the missing
orchestration layer** that turns *thermometer → deep originals → cited brief*,
and adds the discipline that makes the output trustworthy:

> Every brief carries **source_coverage**, **not_covered** (declared blind spots),
> and a **strength verdict** (thick / medium / thin). Thin is labelled thin — never
> dressed up as primary evidence.

---

## Why it's different

| | last30days | agent-reach | **deep-reach** |
|---|---|---|---|
| 30-day trend scan & scoring | ✅ | — | ✅ (delegates) |
| Reads full originals / comments / transcripts | partial | ✅ | ✅ (delegates) |
| Chinese platforms (B站 / 小红书 / 雪球) | weak | ✅ | ✅ |
| Clusters → deep-reads → one cited brief | — | — | ✅ |
| Declares blind spots + evidence strength | — | — | ✅ |
| Free-first; account layer guided & non-resident | n/a | manual | ✅ (see SKILL §5) |

**Selling point in one line:** the comprehensiveness of deep multi-platform
reach *plus* the recency of a 30-day pulse — wrapped in research discipline that
tells you how much to trust the result.

---

## Install

```bash
# 1. Drop the skill into your Claude Code skills dir
git clone https://github.com/Runesmith-Studio/deep-reach.git
cp -r deep-reach/skills/deep-reach ~/.claude/skills/

# 2. (recommended) install the two tools it orchestrates
#    last30days — see https://github.com/mvanhorn/last30days-skill
pipx install agent-reach   # or: pipx install <path-to-clone>   (https://github.com/Panniantong/Agent-Reach)
pipx install bilibili-cli  # optional: login-free Bilibili search

# 3. check what works right now
agent-reach doctor --json
```

deep-reach **degrades gracefully**: with neither tool installed it still produces
a structured, gap-honest brief using Claude's built-in `WebSearch` / `WebFetch`
— just with shallower reach.

## Use

Just ask Claude things like:
- "research what people actually say about X, dig into the originals"
- "把这个话题查实,找几条真实用户原话"
- "competitor research on Y — real user complaints, in their words"

Claude runs the three phases and writes the brief to `./research/`.

---

## 中文简介

`deep-reach` 是一个把**两个开源工具二合一**的 Claude Code 调研 skill,专做
**舆情监测 + 市场调研**:

- **last30days** 当温度计:找出"哪里在讨论、热不热"(Reddit/X/YouTube/HN/GitHub…)。
- **agent-reach** 当深挖手:把原文/评论/视频字幕读全,覆盖 B站/小红书/V2EX/雪球等中文平台。

两个单用都不够——温度计薄且来源易集中,深挖手不打分趋势也不管证据卫生。
**deep-reach 是中间那层指挥**:温度计 → 深挖原文 → 一份可引用简报,并强制
每份简报带 `source_coverage`(覆盖)、`not_covered`(敢标盲区)、厚度判定
(厚/中/薄,薄就标薄、绝不冒充主据)。

**默认零账号**;遇到登录墙挡住最肥证据时,会主动提醒、引导你用一次性专用小号、
用完即清(永不用主账号、永不抽日常浏览器 Cookie,详见 `SKILL.md` §5)。

---

## Credits & License

Built on and grateful to **[last30days](https://github.com/mvanhorn/last30days-skill)**
(Matt Van Horn) and **[agent-reach](https://github.com/Panniantong/Agent-Reach)**.
deep-reach is an orchestration layer; the heavy lifting is theirs.

MIT © Runesmith Studio. Issues & PRs welcome.
