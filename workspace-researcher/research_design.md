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

### 6. Should analysis be a separate program from research? — RESOLVED

**Yes.** Split into `morning_research.md` (gathers, records) and `morning_analysis.md` (forms the
view, logs predictions, writes the report).

The blocking dependency turned out to be a false one. It assumed a split meant *two cron jobs*, and
therefore two sessions with state to pass between them. It does not: **one cron prompt reads both
files in sequence**, so they share a session and the research findings are still in context when the
analysis file is read. State-passing cost is zero, exactly as in the combined version.

The split is worth having because it matches the settled/pending line used throughout the system —
research records what happened, analysis decides what comes next — and it keeps `prediction log`
writes confined to one program.

Note the `weekly_research.md` baseline-storage question does **not** share this answer after all.
That one spans *different days*, so it genuinely needs persisted state and remains open.

### 7. Stop tracking agent-written data in git — once operational

The researcher writes to git-tracked files during normal operation: `logs/*.jsonl`, `sectors.md`,
and `stock_profiles/<code>/`. Deployment is a `git pull` onto the OpenClaw workstation, so after the
system runs, that machine carries local modifications to tracked files and a later pull can conflict
— most often on the append-only logs, where both sides appended different lines.

Target state: git carries only **schemas, templates, playbooks, programs, and seeds**. Agent-written
data lives on a gitignored path.

**Not yet** — while the system is being built, having the data in git is useful: it makes the
agent's output reviewable from the authoring machine and gives history for free. The conflict risk
is near zero until it is actually running daily.

Do this once the morning run is operating on a schedule. When doing it:

- decide whether existing logged data should be preserved or discarded at the cut-over;
- keep an empty seed or `.gitkeep` for each path so the structure still deploys;
- check nothing in `paths.json` assumes the files are present in a fresh checkout;
- until then, if a pull conflicts on a log, **keep both sides' lines in timestamp order** — never
  resolve by taking one side, since append-only means both are real.

### 8. Drift cleanup — files that no longer agree with each other

Found during the 2026-09-25 handover review. Each is a small fix; do them in one pass.

- `research_playbook.md` → *Stock research* still refers to a "market behavior profile". That
  template was removed; its job is now done by `how_it_trades` in `stock_profile.json` plus
  `trading_history.jsonl`.
- `research_playbook.md` → *Stock research* names `refresh_triggers` and `next_suggested_refresh`
  as profile fields. `stock_profile.json` actually has `stock_specific_refresh_triggers` and no
  refresh date; only the financial snapshot has `refresh_control.next_suggested_refresh`.
- `research_playbook.md` → *Methodology* step 5 says to start from the latest weekly baseline. No
  baseline exists yet, and `morning_research.md` never mentions one.
- `morning_research.md` Phase 2 says its classification uses the vocabularies in `sectors readme`,
  but `view_change` and `follow_up_needed` are not defined there.
- `morning_research.md` uses `daily_market_note` as a write label and `daily market notes` as the
  file key. Pick one form.
- `sectors_readme.md`: `move_quality` lists both `unclear` and `unsure`; *Bloat control* mentions a
  "Recent changes" section that is not in the entry format; the optional *Review triggers* section
  is not in the format template, although `sectors.md` includes it.
- `logs_readme.md`: says the logs live in `shared/`; describes `prediction_log` as holding "a stock
  prediction or research note" (it is predictions only, of any target type); the `id` format is
  only implied by the examples.
- `stock_profiles_readme.md` → *How triggers get checked* now says company-specific news must be
  looked up for each tracked stock, but `morning_research.md` Phase 3 has no step that does this.
  Add a per-stock announcement check (cninfo or search) when reviewing that program.
- Open Review Item 3 above (staleness policy) is now mostly answered by
  `stock_profiles_readme.md` → *Staleness*. Check what is left, then close it.

### 9. Log schema gaps to fix before the first run

The logs are append-only, so entries written with a flawed schema cannot be corrected later.

- **Timezone.** Timestamps are UTC, but a 07:30 Shanghai run is 23:30 UTC on the *previous* day.
  IDs, timestamps, and the trading day will disagree. Add a `trading_date` (Shanghai calendar
  date) to every log, or switch timestamps to `+08:00`.
- **Prediction target day.** A horizon of `"open"` does not say which day's open. `trading_date`
  fixes this too.
- **ID scheme.** State the format and how to pick the next sequence number.
- **Outcome verdict.** `market_outcomes` has no hit / miss / partial field, so scoring would depend
  on reading free-text `notes`. Decide before `daily_reflection.md` is written.

### 10. When should `trading_history.jsonl` entries be written?

An entry needs `follow_through`, which is only visible a day or two later, so it cannot be written
on the day itself. Nothing currently makes the later write happen: the morning run notices a
notable day, the session ends, and the day is forgotten.

Candidates, to brainstorm (user, pending):

- the end-of-day review (`daily_reflection.md`) notes the day, and a later one writes the entry;
- a morning run writes it once follow-through is visible, picked up via an `open_question` in
  `daily market notes`;
- a weekly pass writes all of the week's pending entries at once.

### 11. Watch during early runs

Fields and thresholds set by judgment, not evidence. Check each against real output after the first
few weeks.

- **`likely_cause` hit rate** in `trading_history.jsonl`. Count entries by `cause_basis`. How often
  is a cause `sourced` rather than `inferred` or `unknown`? Expectation is low for daily moves.
- **Qualifying-day guide** in `stock_profiles_readme.md` — the 70%-of-daily-limit threshold, and
  whether the agent records too many days or too few.
- **`event_types: other`** — if the `other` entries keep describing the same kind of day, add it
  to the list.

### 12. Design pass: run schedule, and which checks happen in which run

Many rules say *when* something should happen — refresh a profile every 2–4 weeks, write a history
entry once follow-through is visible, re-check valuation monthly — but there is no plan for *which
run* does each. Design the run schedule first, then assign each timed task to a run.

Known or likely runs: morning (pre-open, daily), end-of-day review (`daily_reflection.md`), weekly
research, weekly reflection, and possibly a midday run during the lunch break.

Questions to settle:

- **Event checks vs. time checks.** Event triggers (a new report, a 减持 filing) are found in that
  day's news and must be handled daily. Time-based checks (is this profile overdue?) are not
  urgent. Proposal: the daily run handles events only, and a weekly run handles everything
  time-based.
- **One weekly run or two?** Does a weekly run both find what is due *and* do it, or does it list
  what is due and leave the work to the next runs? Or does it all go in the end-of-week run?
- **Where each timed task goes**, including: profile behavior refresh and consolidation
  (`stock_profiles_readme.md` → *Checking what moves the stock*), financial snapshot refresh,
  pending `trading_history` entries (Item 10), and weekly lesson curation. Note that the behavior
  refresh is the only step that turns `[research]` claims into `[observed]` ones, and no program
  runs it yet.
- **Date fields.** The financial snapshot stores a *next* refresh date; `stock_profile.json`
  stores only *last reviewed* dates and relies on the readme's cadence. Pick one approach for all
  files.
- **Selecting stale stocks** without opening every profile: is a date in `tracked_stocks.json`
  worth the duplication, or is opening each cover page weekly cheap enough?

### 13. Market data ingestion — likely an OpenClaw skill

The end-of-day review needs actual results (index closes, sector rankings, each tracked stock's
price and turnover), and so does seeding `trading_history.jsonl`. How that data reaches the agent
decides what the review can do.

- An LLM reads a **short text digest of pre-computed numbers** well. It handles large raw tables
  poorly (arithmetic over many rows) and chart images unreliably (exact levels).
- So: a script fetches the data (e.g. AKShare for A-shares), computes the comparisons (% change,
  relative to index, turnover vs. its recent average), and hands the agent a digest of about 20
  lines. An OpenClaw skill is the right container for this.
- **Per the OpenClaw docs** (docs.openclaw.ai/tools/skills, checked 2026-10-13): a skill is a folder
  with a `SKILL.md` of instructions, and can bundle supporting scripts. Scripts run through the
  agent's `exec` tool. So Python works, provided two things hold on the workstation — **still to
  check there:**
  1. `exec` is enabled for the researcher agent (set in `openclaw.json`);
  2. Python and the data library are installed — inside the sandbox container if the agent runs
     sandboxed, not just on the host.
- Then run a simple test: ask the agent in a manual session to fetch today's 上证指数 close, a
  sector ranking, and 60 days of one stock's daily prices.
- Prediction horizons must match the data available. If only daily closes can be fetched,
  `morning_session` predictions cannot be scored without a midday run.

### 14. A free-form thinking step in each program

The user wants the agent's own thinking used outside the fixed structure, not only within it.
`research_playbook.md` → *Self Thinking* allows it in general terms, with a `[self-directed]` tag,
but no program gives it a dedicated step. To propose: one explicit free-form step per program,
how its output is recorded, and how `weekly_reflection.md` compares it with the scripted output.

### 15. Full clarity rewrite of the guidance files

Much of the guidance was written by an earlier session in compressed, aphoristic wording ("a
checklist applied in reverse"). Line-by-line patches risk keeping logic nobody actually holds, so
rewrite each file section by section instead.

**When:** after the run-timing design pass (Item 12), since that may move sections between files.
Files already reviewed with the user can be done first; `stock_profiles_readme.md` is one.

**How, per section:**

1. State the section's purpose in one plain sentence.
2. Keep only the rules that serve that purpose. Rewrite each one plainly, with an example where it
   helps.
3. **Do not change the logic.** If a section's logic looks wrong, flag it to the user and agree on
   the fix before rewriting — the user needs to keep track of how the whole system fits together.
4. Replace vague pointers ("X explains this", "see X") with either an explicit instruction saying
   when to read the file, or a one-line summary inline, with the full rule left in its home file.
   A pointer loads nothing by itself; the agent has to decide to open the file.
5. Show the user each file's diff before moving on to the next.

**Order:** the files the morning run reads first — `stock_profiles_readme.md`,
`morning_research.md`, `morning_analysis.md`, `logs_readme.md`, `sectors_readme.md`,
`research_playbook.md` — then the rest.
