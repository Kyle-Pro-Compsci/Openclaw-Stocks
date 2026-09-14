# Morning Analysis Run

## Purpose

Turn the morning's research into a view. This program decides **what is likely to happen today**,
logs the calls that are specific enough to score, and produces the single consolidated report.

Runs immediately after `morning_research.md`, in the same session, with that run's findings still in
context. It does not re-gather — if something is missing, note the gap rather than starting a new
research pass.

## Scope: analysis, not research

`morning_research.md` established **what happened**. This program decides **what it means and what
comes next** — the settled/pending line used throughout this system.

That makes this the run that writes to `prediction log`. The research run does not.

If a genuine gap appears — a claim the stance depends on that was never verified — do a targeted
check rather than a general scan, and say in the report that it was needed. A pattern of these means
`morning_research.md` is missing something and should be adjusted.

## File responsibility

* `research_playbook.md` → *Taking a stance* — how to form a view, when to commit, when to abstain
* `research_playbook.md` → *Prediction quality* — what makes a prediction worth logging
* `logs readme` — the `prediction log` schema
* `paths.json` — where everything lives

## Phase 1: Assemble the picture

From the research run, pull together:

* the macro/geopolitical context and what it implies for risk appetite;
* the A-share regime read;
* sector/theme findings and their classifications;
* affected tracked stocks and what was found for each;
* open questions carried in, and whether today resolved them.

Separate what is **established** from what is **inferred**. A stance built on an inference is fine —
a stance that mistakes an inference for a fact is not.

## Phase 2: Form the view

Follow `research_playbook.md` → *Taking a stance*.

Work at three levels, but only where there is something to say:

1. **Market** — how is the A-share market likely to open and trade today?
2. **Sector/theme** — which are likely to lead, lag, or reverse, and on what conditions?
3. **Stock** — for tracked stocks with something genuinely new.

For each view, be explicit about the **conditions it depends on**, so it can be checked during the
day rather than only judged afterwards.

**Rank by conviction.** Say which call you would act on first.

**Abstaining is a valid outcome.** If the evidence is thin, say "no clear directional view today,
and here is what would create one". Do not manufacture a call to fill the section.

## Phase 3: Log predictions

Only stances specific and falsifiable enough to be scored later go in `prediction log`. Follow
`logs readme` for the schema.

A logged prediction needs a target, a horizon, an expected move or scenario, the thesis and
evidence, a confidence, an invalidation condition, and what to check later.

Views too broad to score still belong in the report — label them as **views**, not logged
predictions. The distinction matters: the reflection loop scores the log, so anything unscoreable
sitting in it will quietly distort the accuracy record.

Do not force a prediction from weak evidence. Zero logged predictions on a quiet day is a correct
outcome, not a failed run.

## Phase 4: Report

One consolidated report, supervisor-ready:

1. **Morning context** — macro/geopolitical, what changed, why it matters
2. **A-share regime** — the likely setup today
3. **Sector/theme view** — where the action is, and the conditions attached
4. **Tracked stocks** — what is new, and what it implies
5. **The call** — the day's stance, ranked by conviction. Abstentions stated as such
6. **Predictions logged** — with their invalidation conditions
7. **Watchlist** — what to check during the day, and what would change the view
8. **Gaps and uncertainty** — blocked sources, unverified claims, stale profiles, what could not be
   established
9. **File updates** — what was written, and what was flagged but not done

Label **facts**, **interpretation**, **prediction**, and **uncertainty** distinctly throughout.

Avoid generic market commentary. If nothing material changed, say so plainly — a quiet morning
reported honestly is more useful than filler.

## Read-check

At the end of the report, list every `READ-CHECK` token encountered across both runs — see
`research_playbook.md` → *Read-check tokens*.

<!-- READ-CHECK: RC-ANALYSIS-V7H2 -->
