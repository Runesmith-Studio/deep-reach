---
name: deep-reach
description: >
  One skill for AI social listening, market research & competitive intelligence —
  turn any question into a cited, gap-honest research brief. Scans the last-30-day
  pulse, reads the real originals across 13+ platforms (Reddit, X, YouTube,
  GitHub, Bilibili, XiaoHongShu, V2EX, web), and synthesizes a brief that carries
  source_coverage, declared blind spots (not_covered), and a trust rating
  (thick / medium / thin) — so a thin hunch is never mistaken for a hard finding.

  一个技能搞定舆情监测 / 市场调研 / 竞品情报 —— 问一句,拿回一份带引用、敢标盲区、带可信度评级的调研简报。

  MUST USE when: public-opinion monitoring / 舆情监测, market & competitor research /
  市场竞品调研, product or niche validation / 选品验证, finding real user quotes &
  pain points / 找真实用户原话与痛点, "verify this for real" / "把这个查实",
  "dig deeper than a quick trend scan" / "挖深一点".

  Under the hood (operational note, not the pitch): deep-reach runs a three-phase
  pipeline and can drive two optional engines when present — a 30-day trend
  thermometer (last30days) for Phase 1 discovery, and a multi-platform reader
  (agent-reach) for Phase 2 deep reads. Both optional; it degrades to the host's
  web search/fetch and always declares what it could not cover.

  NOT for: reading a single URL when you just want raw fetch / generic report,
  slide, or translation production unrelated to evidence gathering. (Its final
  artifact IS a cited brief, but it does not do downstream content production.)
---

# deep-reach — Thermometer → Deep Originals → Cited Brief

> One line: last30days is the radar that spots blips; deep-reach flies to each
> blip, reads the original, and writes it up as citable evidence.
>
> **Free, zero-account, public channels only.** deep-reach reads what's openly
> reachable — no logins, no accounts, no cookies. Anything behind a login wall is
> out of scope and recorded in `not_covered` rather than worked around.

---

## 0. Red lines (always enforced)

1. **Zero-account, public channels only.** Use only the free, no-login channels
   in the table below. **Never** run `agent-reach configure --from-browser` or
   `agent-reach install --channels=...` (both import cookies).
2. **Login-walled platforms are out of scope.** X/Twitter, XiaoHongShu, Xueqiu,
   LinkedIn, and full Reddit comment trees sit behind logins — deep-reach does
   **not** work around them. Record them in `not_covered`; never scrape behind a
   login or touch a main account's cookies.
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
# Invoke the last30days skill:  /last30days <topic>
# Or run its engine directly (there is no guaranteed global `last30days` binary):
#   python3 <last30days-skill-dir>/scripts/last30days.py '<topic>' --emit=json > /tmp/deep-reach-l30d.json
# (run after last30days' own preflight; --emit=json is the engine's stable flag)
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
| Reddit | via the **last30days** engine (it may return Reddit threads/comments in its own pipeline — record whatever coverage/warnings it reports). **agent-reach**'s direct Reddit route is login-backed only (no zero-config path); anonymous `.json`/jina is unreliable. | none (via last30days) |
| YouTube transcript | `yt-dlp --write-sub --write-auto-sub --skip-download -o "/tmp/%(id)s" "<URL>"` | none |
| GitHub repos/issues/discussions | `gh search ...` / `gh issue list` / built-in `WebFetch` | none |
| Bilibili search/detail | `bili search "<query>" --type video -n 5` | none (login-free) |
| V2EX hot/topics | `curl -s "https://www.v2ex.com/api/topics/hot.json" -H "User-Agent: deep-reach/1.0"` | none (may need proxy) |
| Web semantic search (optional) | `mcporter call 'exa.web_search_exa(query: "...", numResults: 5)'` | free Exa key (off by default; if missing, skip and log in not_covered) |

**Discipline**: before digging, run `agent-reach doctor --json` once to see which
channels actually work right now; log the dead ones in `not_covered`, don't
hammer them.

> **Graceful degradation**: deep-reach works best with both last30days and
> agent-reach installed; each phase falls back to the host's `WebSearch` /
> `WebFetch` if a tool is missing. If the host exposes **neither** those tools
> nor the two skills, stop with `not_covered` + setup notes — don't pretend to
> research.

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
- [ ] **Zero-account only** — every source is a public, no-login channel; anything
      login-walled is recorded in `not_covered`, not worked around.
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
  Both optional — deep-reach degrades to the host's `WebSearch`/`WebFetch` when
  available (and stops honestly when not).
- **Does not**: process content beyond gathering & structuring (no report
  writing / slides / translation); no posting/liking/commenting (write actions);
  no reading behind login walls (out of scope → `not_covered`).
- **Uninstall the tool layer**: `pipx uninstall agent-reach bilibili-cli`.

---

## Credits

deep-reach is an orchestration layer. The heavy lifting belongs to two excellent
open tools it builds on — please credit them too:
- **last30days** — the 30-day community thermometer.
- **agent-reach** — multi-platform reach across 13+ sources, free channels first.

MIT licensed. Issues & PRs welcome.
