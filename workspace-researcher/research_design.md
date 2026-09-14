# Research Design Notes

This file is a design scratchpad, not an operational instruction file.

Do not use this file during normal morning research runs unless the user is actively redesigning the workflow.

This file captures brainstorms, design ideas, and open questions for the researcher workflow. It is not operational procedure — see `research_playbook.md` for that.

## Research Categories (Proposed)

1. World / Geopolitical news
2. Financial / market news
3. Financial company info

## Scope and Narrowing

- Start from a wide scope (tech, 科创50) and then narrow down to specific sectors and stocks (e.g., semiconductors).
- To be determined: should scope narrowing be decided in instructions or by the AI?

## Caching Strategy

- Split research between financial data behind the company and current events that affect the market.
- Cache stock info so it does not have to be looked up again unless:
  - New news comes out relating to it, or
  - A certain review period has passed.

## Market Phase Tracker

- What is the phase of the market?
  - Consolidation phase / 震荡阶段?
  - Bull or bear market?
- Keep a running tracker.
- Predict when it might shift; look for news/evidence where a shift could occur.

## Chaining Searches

- When the research finds a thread of note, chain multiple searches to dig deeper.
- Combine with weekly research as a baseline, then use daily research to update.

## Weekly Baseline

- Maintain weekly research results as a baseline.
- Start daily research from the latest weekly baseline.

## Decision Style

- Choose between giving cautious results or actually taking a side.
- Example: predict whether the market will open high or low and how it will go.

## Analyst Process Goal

- Ideally replicate the same process a real analyst would do for a stock trading firm.
- Current status: unknown — needs research or expert input.

## Validation Rule

- First validate that information saved from before (e.g., market hotspots) is still valid before building on it.

## User vs. AI Notes Separation

- Analysis notes created from self-improvement reflection should be separated from user-created notes and instructions.
- Example user instructions: pay attention to Japanese markets, gold prices, Trump statements, etc.

## Temp Notes (Legacy)

These items were moved from `research.md` and `temp_notes.md` during v0 scaffold cleanup.

---

## Open Review Items

Deferred design reviews, addressed to whoever is maintaining this repo (a Claude Code session or
the user) — **not** to the OpenClaw researcher agent at runtime. Do not act on these during a
normal research run.

### 1. Audit the readme-before-write rule across all documents

`AGENTS.md` now says: read a readme before **writing** to its file, but not merely to read the
file. That rule was tightened from "always read the readme first before reading or especially
writing" because the old version forced a heavy readme load on every data-file read.

Still to check, file by file:

- Does reading each data file **without** its readme context actually make sense? Some readmes
  carry interpretation rules, not just write rules — `sectors_readme.md` defines the controlled
  vocabulary (`heat_status`, `move_quality`, `stage`) that gives `sectors.md` entries their
  meaning. Reading `sectors.md` cold may be fine (the values are fairly self-describing) or may
  cause misreading. Verify per file rather than assuming.
- Which readmes are genuinely write-only guidance (`logs_readme.md` looks like it — pure schema)
  versus read-and-write guidance (`sectors_readme.md`, `stock_profiles_readme.md` may be).
- If a readme turns out to be needed for *reading*, the fix is probably to move the small
  interpretation part into the data file's own header — the way `learned_research_lessons.md`
  carries its read-weighting rules inline — and leave the bulk write rules in the readme.
- Confirm no program still assumes the old "read readme before reading" behavior.

### 2. End-to-end walkthrough of a hypothetical run

Before trusting the cron job, trace one complete morning run from start to finish on paper,
reasoning as OpenClaw would actually execute it, not as the documents intend:

- What is auto-injected at session start versus what must be explicitly read? (Known: startup
  context provides `AGENTS.md`, `SOUL.md`, `USER.md`, and memory. `shared_files/` is **not**
  auto-injected and must be read via tool calls through `paths.json`.)
- At each phase, what is actually in context at that moment? Does the agent have what the phase
  assumes it has?
- Where does the instruction chain rely on the agent remembering something read many steps
  earlier, and is that realistic?
- Where do two instructions conflict, so behavior becomes unpredictable?
- Does the run stay within a sane context budget, or does it accumulate every shared file?
- What happens on failure paths: file missing, search blocked, empty `tracked_stocks.json`,
  a sector absent from `sectors.md`?
- Does anything require a capability OpenClaw may not have (real-time quotes, historical price
  series, intraday data)? The market behavior profile in particular assumes access to recent
  price/volume history — confirm that data is actually obtainable before relying on it.

Known OpenClaw limitations to keep in mind during the walkthrough are recorded in `../CLAUDE.md`
under *Unverified OpenClaw behavior*; resolve those first, since several of these questions depend
on them.

### 3. Define the cache staleness policy for stock profiles

The *mechanism* exists but the *policy* does not. `stock_profile.json` has `cache_status`
(`last_full_profile_refresh`, `last_daily_check`, `sections_needing_refresh`) and a `last_*_update`
per digest; both heavy templates have `refresh_control.next_suggested_refresh` (time trigger) and
`refresh_control.refresh_triggers` (event trigger).

Missing:

- **A concrete cadence.** How long before a financial snapshot is stale — a quarter, tied to
  reporting season, or a fixed number of weeks? Valuation probably goes stale faster than
  financials; market behavior faster still.
- **Wiring.** Nothing currently instructs the morning run to *read* these fields and act on them.
  Phase 4 should check both triggers before trusting cached numbers.
- **Where the policy is written.** `stock_profiles_readme.md` is the right home.

### 4. Review the sector and stock research methodology as a whole

The current methodology is good at building a general picture — heat status, current view, drivers
— but was written descriptively and does not always land on something actionable. A `Taking a
stance` section has been added to `research_playbook.md`, and the sector and stock sections now end
on "what does this imply for today", but the underlying methodology deserves a proper review pass
rather than a bolted-on conclusion step.

Worth asking: is the sector entry format collecting the fields that actually drive a directional
call, or just the ones that are easy to describe?

### 5. Let the analysis phase write back to sector profiles

Currently `sectors.md` updates are framed as evidence-driven: news arrives, the sector view changes.
But the **analysis** step can also produce a reason to update — having reasoned through the day's
picture, the agent may conclude that a sector's recorded move quality, stage, or driver is wrong
even though no single new fact contradicted it.

To add:

- A path in the morning run's write-decision phase for analysis-originated sector updates, not only
  evidence-originated ones.
- A matching allowance in `sectors_readme.md`, whose update triggers currently read as
  externally-caused only.
- Keep the distinction visible in the entry — an update from reasoning is weaker evidence than one
  from an observed fact, and should be marked as such so it can be revisited.

### 6. Should analysis be a separate program from research?

Currently the morning run gathers *and* concludes. The alternative is a `morning_analysis.md`
chained after `morning_research.md`.

**Blocking dependency:** a split only works if both programs run in the **same session**, or if the
research program persists enough state for the analysis to reconstruct. If a second cron fires a
fresh session, the analysis program would have to rebuild the whole morning's context from
`daily_market_notes.jsonl` — expensive and lossy. Resolve the cron session-semantics question in
`../CLAUDE.md` first.

Decision for now: analysis stays as a phase inside `morning_research.md`, where the research is
already in context and the state-passing cost is zero. Revisit once session semantics are known —
note that the `weekly_research.md` baseline-storage question depends on the same answer, so the two
should be decided together.
