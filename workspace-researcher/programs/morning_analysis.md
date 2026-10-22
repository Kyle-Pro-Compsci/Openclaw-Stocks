# Morning Analysis Run

## Purpose

Turn the morning's research into a view. This program decides **what is likely to happen today**,
logs the calls that are specific enough to score, and sends the **analysis report**. The research
report has already been sent by `morning_research.md`.

Runs immediately after `morning_research.md`, in the same session. Its input is the `## Handoff`
section of today's `run notes`. It does not re-gather — if something is missing, note the gap rather
than starting a new research pass. The one planned exception is the *Live check* below.

## Scope: analysis, not research

`morning_research.md` established **what happened**. This program decides **what it means and what
comes next** — the settled/pending line used throughout this system.

That makes this the run that writes to `prediction log`, and the run that writes `view` notes to
`daily market notes`. The research run does neither.

If a genuine gap appears — a claim the stance depends on that was never verified — do a targeted
check rather than a general scan, and say in the report that it was needed. A pattern of these means
`morning_research.md` is missing something and should be adjusted.

## Where the rules live

This program sets the order of work only. How to form a view, and when to abstain, is in
`research_playbook.md` → *Taking a stance* and *Prediction quality*. The schemas for both logs are
in `logs readme` — read it before writing (Phase 3).

## Live check: Asian opens

The research covered everything up to the start of today's session. Do this check first, as late in
the run as possible, so the markets have had time to trade.

Check how **KOSPI** and the **Nikkei 225** have opened (both open at 08:00 Shanghai time): the move
since open, and anything notable — for example Samsung and SK Hynix for the semiconductor read.
Use at most two searches. Append the results, with sources and the time of each figure, to `run
notes` under `## Asian opens`. If a figure cannot be found, say so — do not use the previous
session's close as if it were today's open.

These feed Phase 2 (market) and Phase 3 (sectors).

## Phase 0: Prior positions

Before analysing today, collect what this system already thinks. These are the positions today's
calls will confirm, revise, or reverse.

**Sector views and stock narratives:** take them from `run notes`. The research recorded each one as
it stood *before* this morning's updates — those are the prior positions. The files on disk now
hold today's updated versions, so they cannot serve as "before".

**Read fresh from the files:**

* `prediction log` — open predictions (horizon not yet passed);
* `market outcomes` — the results of recent predictions, matched to them by `linked_prediction_id`.
  The prediction log itself is never edited; outcomes only ever live here;
* `daily market notes` — the most recent run's `view` notes;
* `learned research lessons` — the lessons that could apply to today's setup;
* `sectors` — the current entry for each sector in the handoff, i.e. today's updated state (heat,
  stage, move quality, watch points);
* the `stock_profile.json` of each stock in the handoff — today's working detail (key levels, news
  filter, recent activity, upcoming events).

So the analysis holds three things for each sector and stock: what we thought **before** today (from
`run notes`), what the research **changed** this morning (the current files), and the call it is
about to make. Comparing the first with today's facts is how Phase 1 finds what changed.

Append a `## Prior positions` section to today's `run notes`, one line per item: what we thought
before today, and since when. Phase 1 works from this section and the handoff.

## Phase 1: Assemble the picture

Read the `## Handoff` and `## Prior positions` sections of today's `run notes`. Work from them, not
from memory. Re-open another file only if a call depends on something both sections lack, and say
so in the report.

Sort what you have into three groups:

1. **Established** — facts from the research, each with its source.
2. **Inferred** — readings the research made but did not confirm.
3. **Prior positions** — from Phase 0.

A call built on an inference is fine; a call that mistakes an inference for a fact is not.

Then list **what changed against our prior positions**. That difference is what this program acts
on. If nothing changed, the prior positions largely stand — say so, and say why.

## Forming calls (Phases 2–4)

Phases 2–4 work top-down: market, then sectors, then stocks. Each level must agree with the level
above it, or say why it does not. Follow `research_playbook.md` → *Taking a stance*: every call says
which way you expect things to go and the conditions it depends on, so it can be checked during the
day.

**Abstaining is a valid outcome** at any level. If the evidence is thin, say "no clear view today,
and here is what would create one". Do not manufacture a call to fill the section.

### Prediction or view

For each call, judge whether you can state it **precisely**: a target, a measurable threshold, a
time, and data that will exist to settle it.

* If you can — without inventing a number you do not actually believe — it is a **prediction**.
* If you cannot, it is a **view**. Say it in plain words; it needs no threshold or score.

| Call | Kind |
|---|---|
| "Semiconductor index outperforms 上证指数 by the 11:30 close" | Prediction |
| "上证指数 closes up on the day" | Prediction |
| "Semis look strong while the overseas lead holds" | View |
| "This rally feels late" | View |

**Decide by precision, not by confidence.** A precise call you hold with low confidence is still a
prediction — record the low confidence. If only confident calls were logged, the system could never
check whether its confidence means anything.

**These are always predictions:** the market call (stated against an index, e.g. 上证指数, full
day), and each sector call from Phase 3. If you cannot state one of them precisely, abstain
explicitly and say why — do not quietly turn it into a view.

Use judgment at the boundary. When unsure, ask: "Tomorrow, could someone check this against the data
and say right or wrong without arguing about what I meant?" If yes, it is a prediction.

## Phase 2: Market

Start from the macro developments and the market regime. For each development that matters, trace
how it reaches A-shares:

* **Directly** — the sectors it hits (Hormuz → oil, shipping).
* **Indirectly** — the knock-on effects (instability → gold; higher oil → airline and chemical
  costs).
* **Through policy and money** — expectations of state support, liquidity, northbound flows, the
  yuan.
* **Through mood** — whether it shifts overall risk appetite.

Follow only the routes that actually apply today. One strong route beats five weak ones.

Conclude with a market call: how the market is likely to open and trade today, or an abstention.

## Phase 3: Sectors

For each sector in the handoff, combine today's market call, the routes from Phase 2 that reach it,
and its current state — heat, stage, move quality. Is it likely to lead, lag, or reverse today, and
on what conditions?

If a sector call goes against the market call — a strong sector in a weak market — say why. If it
differs from the sector's prior view (from *Prior positions*), say what changed.

## Phase 4: Stocks

By `tracking` value (see `tracked stocks readme`):

* **`active` — every one gets a read, every day.** Combine the sector call with the stock's profile
  — how it trades, key levels, news filter, recent activity, upcoming events. Is it likely to follow
  its sector, diverge, or react to its own news?
* **`passive` — only if the research selected it today**, because something touched it. Same
  treatment as an active stock. Otherwise leave it out.
* **`off`** — never included.

**A quiet active stock still gets a read.** Several days without news is information. Is it holding up
against its sector, or lagging? Is it sitting near a key level, going quiet ahead of an upcoming
event, or is its narrative going unconfirmed for longer than it should? Say which, and if the
evidence for that read is thin — for example, no price data — say so.

## For every call: why

Under each call from Phases 2–4, state in a line or two:

* the evidence it rests on, and whether that evidence is established or inferred;
* which lesson or past call, right or wrong, shaped it — if any;
* what would make it wrong.

Until scored outcomes exist, say "no track record yet" rather than leaving the line out.

Then **rank all calls by conviction**: say which you would act on first.

## Phase 5: Log predictions and views

Read `logs readme` before appending — it holds the schema for both logs.

Each call from Phases 2–4 was judged a prediction or a view under *Prediction or view*. Write each
one once:

* **Prediction** → append to `prediction log`, with the thesis, evidence, confidence, invalidation,
  and what to check later.
* **View** → append to `daily market notes` with `kind: view`.

Every call also appears in the report, labelled **prediction** or **view**.

The line matters because the reflection loop scores `prediction log`: one entry that cannot be
scored distorts the accuracy record. Views are still kept, so the thinking behind them can be
reviewed.

Do not force a prediction from weak evidence. Zero logged predictions on a quiet day is a correct
outcome, not a failed run.

## Phase 6: Report

The analysis report, sent after the research report. Do not repeat the research report's news —
the team has just read it. Lead with the call:

1. **The call** — the day's calls, ranked by conviction, each with its *why*. Abstentions stated
   as such
2. **Market regime** — the likely setup today
3. **Sector/theme view** — where the action is likely to be, and the conditions attached
4. **Tracked stocks** — what the findings imply for each
5. **Predictions logged** — with their invalidation conditions
6. **Views noted** — the broader views written as `view` notes
7. **Watchlist** — what to check during the day, and what would change the view
8. **Gaps and uncertainty** — blocked sources, unverified claims, stale profiles, what could not be
   established
9. **File updates** — what this program wrote

Label **facts**, **interpretation**, **prediction**, and **uncertainty** distinctly throughout.

Avoid generic market commentary. If nothing material changed, say so plainly — a quiet morning
reported honestly is more useful than filler.

## Read-check

Record any `READ-CHECK` token you see during this program in the `## Read-checks` section of
`run notes`, as the research did. Before each phase of this program, re-read that phase's section.

At the end of the report, list every token from the `## Read-checks` section of `run notes` — see
`research_playbook.md` → *Read-check tokens*.

Then say whether you saw a compaction summary at any point in this session (yes / no). If yes, a
missing token may mean the file's content was summarised away, not that it was never read.

<!-- READ-CHECK: RC-ANALYSIS-V7H2 -->
