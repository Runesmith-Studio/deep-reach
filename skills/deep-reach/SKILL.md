---
name: deep-reach
description: >
  Deep evidence research pipeline — turn a shallow "last-30-days community pulse"
  into a cited, gap-honest research brief. It chains a trend thermometer
  (last30days) with multi-platform deep reading (agent-reach) and adds evidence
  discipline: every brief carries source_coverage, not_covered, and a strength
  verdict (thick / medium / thin).

  深度证据调研流水线 —— 把"近30天社区温度计"深挖成可引用、敢标盲区的证据简报。

  MUST USE when: public-opinion monitoring / 舆情监测, market & competitor research /
  市场竞品调研, product or niche validation / 选品验证, finding real user quotes &
  pain points / 找真实用户原话与痛点, "verify this for real" / "把这个查实",
  "last30days is too thin, dig deeper" / "挖深一点".

  How it relates to the two tools it orchestrates:
  - last30days = the thermometer (finds WHERE a topic is discussed and how hot).
    deep-reach calls it in Phase 1; it does not reimplement it.
  - agent-reach = the raw reach tool (reads one URL / searches one platform,
    incl. Reddit, X, YouTube, GitHub, Bilibili, XiaoHongShu, V2EX, RSS, web).
    deep-reach calls its free channels in Phase 2.
  - deep-reach = the orchestrator that turns "thermometer → deep originals →
    cited brief" into one disciplined pipeline.

  NOT for: a quick trend glance (use last30days directly) / reading a single URL
  (use agent-reach directly) / writing reports, slides, or translations
  (this skill only gathers & structures evidence).
---

# deep-reach — Thermometer → Deep Originals → Cited Brief

> One line: last30days is the radar that spots blips; deep-reach flies to each
> blip, reads the original, and writes it up as citable evidence.
>
> **Free, zero-account channels by default.** An account layer exists but is
> *not* always-on — when a paywall/login blocks the richest evidence, deep-reach
> *prompts you*, then *guides* a safe, throwaway setup (see §5). Never your main
> account, never your daily browser's cookies.

---

## 0. Red lines (always enforced)

1. **Zero-account by default.** Without the §5 prompt-and-approve flow, use only
   the free-channel table below. By default **never** run
   `agent-reach configure --from-browser`, **never** run
   `agent-reach install --channels=...` (it triggers cookie import).
2. **Account layer = on-demand + guided + per-use approval + non-resident**
   (not forbidden, just controlled). Login-walled platforms (X/Twitter,
   XiaoHongShu, Xueqiu, LinkedIn, full Reddit comment trees) are off by default.
   When the richest evidence sits behind a login, follow §5: prompt the user
   ("what extra depth this unlocks + the cost") → approval → guide a dedicated
   throwaway account → clear credentials after. **Never** auto-enable, **never**
   use the user's main account, **never** pull cookies from the daily browser,
   **never** commit cookies to a repo.
3. **Declare your blind spots.** Every brief carries `not_covered` (what was not
   checked + why). Out of reach ≠ pretend covered.
4. **Thin is thin.** Evidence concentrated in a single source / total < 5 items →
   mark `strength: thin` at the top and say plainly "don't treat as primary".

---

## 1. The three-phase pipeline

### Phase 1 — Thermometer (call last30days)

Invoke the `last30days` skill (or its CLI) to get two things:
- **Hot topic clusters**: which sub-topics are active, and the direction of talk.
- **A batch of source URLs**: Reddit/HN/YouTube/GitHub post links, plus its own
  `source_coverage` / `warnings`.

```bash
# Structured output (URLs + coverage) to feed Phase 2.
# Path depends on your last30days install; --emit=json is the stable contract.
last30days "<topic>" --emit=json > /tmp/deep-reach-l30d.json
```

Note the `warnings` last30days reports itself (e.g. "evidence highly
concentrated" / "Sources: none") — those are exactly the gaps Phase 2 fills.

> **Phase 1 is skippable.** Run the thermometer only for trend-discovery / "is
> this hot right now" tasks. For **targeted evidence research** (nail down a
> stable topic, find specific quotes, pull regulatory/legal text — not a social
> trend), skip straight to Phase 2 and note in `source_coverage` that
> last30days was not run, with the reason.

### Phase 2 — Deep originals (free channels, zero-account)

For the **thin / concentrated / snippet-only** signals from Phase 1, pull the
full originals. Prefer tools you already have; reach for agent-reach's free
channels for the Chinese/niche platforms your built-ins can't touch.

**Free-channel table (use only these by default):**

| What to dig | Command | Account? |
|---|---|---|
| Any web page (clean text) | `curl -s "https://r.jina.ai/<URL>"` | none |
| Any web page (fallback) | built-in `WebFetch` | none |
| Reddit post body | via **last30days backend** (direct `WebFetch`/jina of `.json` is blocked by Reddit as of 2026-06); full comment trees need login = out of scope | none |
| YouTube transcript | `yt-dlp --write-sub --write-auto-sub --skip-download -o "/tmp/%(id)s" "<URL>"` | none |
| GitHub repos/issues/discussions | `gh search ...` / `gh issue list` / built-in `WebFetch` | none |
| Bilibili search/detail | `bili search "<query>" --type video -n 5` | none (login-free) |
| V2EX hot/topics | `curl -s "https://www.v2ex.com/api/topics/hot.json" -H "User-Agent: deep-reach/1.0"` | none (may need proxy) |
| Web semantic search (optional) | `mcporter call 'exa.web_search_exa(query: "...", numResults: 5)'` | free Exa key (off by default; if missing, skip and log in not_covered) |

**Discipline**: before digging, run `agent-reach doctor --json` once to see which
channels actually work right now; log the dead ones in `not_covered`, don't
hammer them.

> **Graceful degradation**: deep-reach works best with both last30days and
> agent-reach installed, but each phase falls back to built-in `WebSearch` /
> `WebFetch` if a tool is missing. With neither tool you still get a structured,
> gap-honest brief — just shallower reach.

### Phase 3 — Synthesize into a cited brief

Write the brief with the template below. Default output:
`./research/deep-reach-<topic>-<date>.md` (or wherever your project keeps research).

```markdown
# deep-reach evidence brief: <topic>
> date <YYYY-MM-DD> · strength: <thick / medium / thin>

## TLDR (≤3 lines)
<the key finding + so-what, plain language>

## Source Coverage
- last30days: <sources / date_range / warnings>  (or "not run — <reason>")
- deep channels: <which free channels used, how many items each>

## Evidence matrix
| claim | support/refute/mixed/thin | evidence (quote/summary) | source URL | strength (observed/inferred/thin) |
|---|---|---|---|---|
| <claim> | support | "<user quote>" | <url> | observed |

## not_covered (what was skipped + why)
- <platform/angle>: <reason, e.g. "login required, out of scope" / "Exa key not set" / "V2EX proxy failed">

## Verdict
- <can you conclude / what evidence is still missing / suggested next step>
```

---

## 2. Acceptance bar (self-check after every run)

- [ ] At least **5 full-text evidence items** (not snippets).
- [ ] At least **2 of them non-Reddit** (proof you actually filled the gap, not
      just landed on one source again).
- [ ] The brief explicitly lists `not_covered`.
- [ ] **Zero-account by default**; if the account layer was used, the §5 flow was
      followed (approval + dedicated throwaway account + credentials cleared
      after), and `source_coverage` names which platform account was used.
- [ ] Evidence is **distilled into the brief** (feeds a decision), not dumped raw.

Below 5 items / 2 non-Reddit → mark `strength: thin` and say plainly "didn't dig
enough this round, don't treat as primary."

---

## 3. Typical use cases

- **Public-opinion / 舆情**: pull real user quotes and sentiment on a topic,
  brand, or event — beyond the thin snippet a thermometer gives.
- **Market & competitor research**: dig out competitors' real user complaints
  and praise, in their own words.
- **Product / niche validation**: before committing, gather real demand signals
  and pain points from the people who actually post about it.
- **Keyword / positioning research**: find the exact words users use to describe
  a pain point, to reverse into copy or search terms.
- **Topic deep-dive**: when a trend scan is too thin one week, add a handful of
  full-text originals instead of guessing.

---

## 4. Dependencies & boundaries

- **Best with**: [`last30days`](https://github.com/mvanhorn/last30days-skill)
  (trend thermometer) + [`agent-reach`](https://github.com/Panniantong/Agent-Reach)
  (multi-platform reach, free channels via `bili` / `curl jina` / `yt-dlp` etc.).
  Both optional — deep-reach degrades to built-in `WebSearch`/`WebFetch`.
- **Does not**: process content beyond gathering & structuring (no report
  writing / slides / translation); no posting/liking/commenting (write actions).
  Reading login-walled platforms = controlled, see §5.
- **Uninstall the tool layer**: `pipx uninstall agent-reach bilibili-cli`.

---

## 5. Account layer (on-demand · guided · non-resident)

> Design intent: the account layer is **included but not always-on**. When a
> login wall blocks the richest evidence, deep-reach **prompts** you, and only
> after approval **guides** a safe setup — it does **not** keep credentials
> sitting inside the tool.

### 5.1 When to prompt (not every time)

During Phase 2, prompt the user to consider an account **only if both** hold:
1. The richest evidence is clearly on a login-walled platform (Chinese consumer
   scene → XiaoHongShu; Western indie/SaaS → X; finance → Xueqiu; full Reddit
   comment trees), **and**
2. After the free layer, evidence is still **thin / medium**, and this layer
   would **materially change the verdict** (not just nice-to-have).

If it's merely out of reach but doesn't change the conclusion → don't prompt;
just record it in `not_covered`. Don't open an account for the sake of it.

### 5.2 Prompt wording (plain, 5-second decision)

```
The richest real quotes on this topic are on <platform> (behind a login);
the free layer only got us to <thin/medium> this round.
A dedicated <platform> throwaway account would add: <what specifically>.
Cost: maintaining the account + that platform's anti-bot risk. Open it? (yes/no)
```

No → ship the brief, record the platform in `not_covered`. Yes → go to §5.3.

### 5.3 Guided setup flow (walk the user through it, do not auto-run)

1. **Confirm a dedicated throwaway account** — registered for scraping, **not**
   the user's personal/main account. If none exists, have the user create one.
2. **Log in within an isolated environment** — a separate browser profile /
   sandbox; **never** in the user's daily browser.
3. **Get credentials (either way, never the daily browser)**:
   - paste that account's cookie into the tool (`agent-reach configure`), **or**
   - run `agent-reach configure --from-browser <isolated-profile>` against that
     isolated profile only.
   - ⚠️ **Never** run `--from-browser` against the daily browser (it would pull
     real-account credentials).
4. **Fetch for this task only** — read/search what's needed, distill into the
   brief, note in `source_coverage` that a `<platform>` account was used.
5. **Clear credentials after (non-resident)** — reset that platform's cookie
   when done (`agent-reach configure` reset / remove the entry under
   `~/.agent-reach`). Don't leave a login session living in the tool. Next time,
   run §5.3 again.

### 5.4 Account-layer hard safety lines (always)

- Dedicated throwaway accounts only; never the user's main account on any platform.
- Cookies never committed to a repo / never pasted into a shared brief.
- Never auto-pull cookies from the daily browser.
- Default state (no §5.3 run) = account channels off, shown unconfigured in `doctor`.

---

## Credits

deep-reach is an orchestration layer. The heavy lifting belongs to two excellent
open tools it builds on — please credit them too:
- **last30days** — the 30-day community thermometer.
- **agent-reach** — multi-platform reach across 13+ sources, free channels first.

MIT licensed. Issues & PRs welcome.
