# Weekly Research Run

> **Status: outline / WIP.** The daily `morning_research.md` run is the priority; this program is
> sketched but not yet ready for a cron job. Do not schedule it until it has been walked through
> end to end.

## Purpose

The weekly deep-research pass. Broader and more thorough than the daily run, executed at the start
of the trading week. Its job is to produce the **baseline** that the week's daily runs then build
on and test, so each morning run can ask "what changed?" against something solid rather than
rebuilding context from scratch every day.

This program **researches**. The weekly review of how last week's predictions performed is a
separate program, `weekly_reflection.md`, and should run before this one so its conclusions can
feed in.

## Relationship to the daily run

| | `weekly_research.md` | `morning_research.md` |
|---|---|---|
| Cadence | Start of the trading week | Every trading day, pre-open |
| Scope | Broad, structural | Narrow, "what changed since yesterday" |
| Output | Baseline view + hypotheses for the week | Today's setup and watchlist |
| Sector work | Full review of every tracked sector | Only sectors touched by today's news |

The daily run should start from the latest weekly baseline. If the baseline is stale or missing,
the daily run says so rather than silently substituting its own.

## File responsibility

This program controls flow only. It does not restate write rules:

- `sectors_readme.md` governs when and how `sectors.md` is updated.
- `logs_readme.md` governs JSONL appends.
- `stock_profiles_readme.md` and the templates govern stock profile files.
- `source_guide.md` governs source reliability.
- `paths.json` locates shared files.

## Phases (draft)

### Phase 0 — Inputs

Read `user research priorities`, `learned research lessons`, and last week's entry in
`weekly reflections`. Note any question the reflection flagged for investigation this week.

### Phase 1 — Macro and policy baseline

Establish the structural picture rather than the day's headlines: policy direction, liquidity
conditions, the state of major overseas markets, and any scheduled events in the coming week
(economic releases, policy meetings, earnings dates, index rebalances).

### Phase 2 — Market regime

Assess the A-share regime for the week ahead: bull / bear / consolidation / 震荡, policy-driven,
liquidity-driven, or theme-driven. State what would signal a regime change, since this tag is what
the market-phase weighting in `learned research lessons` keys off.

### Phase 3 — Full sector review

Unlike the daily run, review **every** sector in `sectors.md`, not only the ones in today's news.
Apply the review test in `sectors_readme.md` to each. This is the run where stale sector entries
get corrected and genuinely new sectors get added.

### Phase 4 — Tracked stock review

Review the watchlist as a whole: which stocks have gone stale, which profiles are missing, which
have upcoming catalysts (earnings, lock-up expiries, announcements) during the week.

### Phase 5 — Hypotheses for the week

Produce a small set of explicit, testable hypotheses for the daily runs to check against — for
example "semis lead if overseas AI hardware stays strong" or "gold fades if the dollar firms".
These are not predictions for the prediction log unless they are specific and falsifiable enough
to qualify; the point is to give the daily runs something concrete to confirm or contradict.

### Phase 6 — Output

A baseline report the daily runs can reference: macro backdrop, regime, sector map, stocks needing
attention, hypotheses, and the week's scheduled events.

## Open questions

- Where should the weekly baseline be stored so the daily run can read it cheaply? Options: a
  dedicated `weekly_baseline.md` shared file, or reconstructing it from `sectors.md` plus the last
  `weekly reflections` entry. A dedicated file is probably clearer but adds another shared file to
  maintain.
- How much of Phase 3 is affordable in one run once the sector list grows?
- Should this run on Sunday evening or Monday pre-open?
