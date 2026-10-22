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

## Considered features

Ideas deliberately not built yet. Each says what would make it worth doing.

- **High / normal priority within `active` stocks.** Different handling for the most important active
  stocks — e.g. deeper daily checks — assigned manually or automatically by how much a stock has
  been moving. Worth doing only once the two would actually be handled differently.
- **One playbook for the whole morning run.** `research_playbook.md` already holds analysis
  methodology too (*Taking a stance*, *Prediction quality*). It could become a single
  morning-run playbook, or be split into research and analysis playbooks. Not urgent; revisit
  during the clarity rewrite (Item 15).
- **Coded skills for mechanical steps** — data fetching, the full-watchlist announcement check,
  removing past `upcoming_events`, counting. Cuts tokens once the steps are stable (see Item 13).
- **Daily summary note for steady days** — one `type: summary` observation per run, so a quiet
  market is recorded in the logs and not only in the report. (Now planned for the post-close run;
  see Item 12.)

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
- `sectors_readme.md`: `move_quality` lists both `unclear` and `unsure`; *Bloat control* mentions a
  "Recent changes" section that is not in the entry format; the optional *Review triggers* section
  is not in the format template, although `sectors.md` includes it.
- `logs_readme.md`: says the logs live in `shared/`; describes `prediction_log` as holding "a stock
  prediction or research note" (it is predictions only, of any target type); the `id` format is
  only implied by the examples.
- `stock_profiles_readme.md` → *How triggers get checked* says company-specific news must be looked
  up for each tracked stock; `morning_research.md` Phase 3 now checks `priority: high` stocks only.
  Reword the readme to match, or close the gap with the full watchlist check in Item 13.
- `stock_profile_template.json` → `identity.status` (default `"active"`) can now be confused with
  `tracking` in `tracked_stocks.json`. Decide what it means — listing status (listed / suspended /
  ST / delisted) would be useful — and rename or remove it. `tracking` is the only place that says
  how closely a stock is followed.
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

**Decided (2026-10-20): write the entry at the next day's close.** For a notable day D:

1. **Post-close run on D** — records the day in the profile's `recent_activity` with
   `follow_through: pending`.
2. **Post-close run on D+1** — fills in `follow_through` (did it gap, hold, fade, or extend?) and
   writes the `trading_history.jsonl` entry.

Why D+1: in A-shares whether a big move holds is mostly settled by the next session (T+1
settlement means first-day buyers can only sell on day two). D alone is too early. D+2 mainly
matters for limit-up streaks, but if D+1 is itself notable it gets its own entry, so a streak
appears as a run of entries anyway.

**Not yet applied to the programs:** `morning_research.md` Phase 5 still says the morning run
records the day in `recent_activity`. That moves to the post-close run once it is written; the
morning run only reads `recent_activity`.

**Seeding a new stock** is separate: filling `recent_activity` and `trading_history.jsonl` for a
newly tracked stock means looking back over previous days and weeks, which needs historical price
data — the market data skill (Item 13), or patchy web search until then. `stock_profiles_readme.md`
→ *Seeding history* already says to do it only if the data is genuinely retrievable.

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

User input (2026-10-19):

- **Runs wanted:** every trading day — a morning run (research + analysis), possibly a midday
  check on how the market is going and whether to adjust, and a post-close run for reflection and
  self-improvement. The post-close run may **propose** edits to program files (to try it). Weekly
  or monthly: more extensive runs — deeper stock research, deeper reflection, and more to be
  worked out.
- **Deadlines:** the morning report must be out by **09:00** Shanghai. The midday run must start and
  finish within **11:30–13:00**.
- **Cost:** results matter more than cost, but write efficiently. Later, program-like parts can be
  replaced by coded skills to cut tokens.
- **Watchlist:** 5–20 stocks, with tiers — some watched actively, some reported only when movement
  is detected.
- **Trading history timing (Item 10):** decided — the entry is written at the next day's close.
  See Item 10.

Decisions and proposals (2026-10-20):

- **Morning run: one cron job, starting 08:00**, with the KOSPI and Nikkei opens (both 08:00
  Shanghai) read as the **last research step before Phase 5** (decide and write), so they have had
  time to trade. A two-cron split was considered and rejected: one job could start before the other
  finished, or run after the first failed. **Timing risk:** 08:00 → report by 09:00 is one hour for
  research and analysis. Measure the duration in the manual run; if too slow, start earlier and keep
  the Asia step late, or revisit.
- **Hong Kong opens at 09:30**, after the morning report, so HK spillover can only come from the
  previous session and futures/ADRs.
- **Post-close: two parts (proposed).** (1) **~15:10 results** — closing figures are on data sites
  within minutes of the 15:00 close: score the day's predictions, record notable days, flags, and a
  quick Feishu report for people waiting on results. (2) **Evening reflection, ~18:00 or later** —
  self-improvement and proposed edits, after 龙虎榜 (the exchange's list of top buyers/sellers on
  unusual stocks, published late afternoon) and most company announcements are out. No one is
  waiting on it.
- **What the morning run depends on from the previous post-close run** — make each an explicit
  post-close task:
  - `recent_activity` for each notable stock (follow-through pending);
  - `key_levels` updated in the profile if a known level broke;
  - a `type: summary` observation in `daily market notes`: how the market and each watched sector
    closed — otherwise the morning run has no record of yesterday's close;
  - `market outcomes` entries for the day's predictions (analysis Phase 0 reads them);
  - `open_question` flags for abnormal moves.

  If the post-close run failed or was skipped, the morning run must say so and look up yesterday's
  close itself. Evening announcements are caught by the morning run's announcement check.
- **Midday report (Feishu):** how the morning went — the overall market, each watched sector, each
  stock; how that compares with what the morning run expected; and an updated afternoon call if the
  view changed.
- **Program edits from reflection: propose only.** Proposals go to a dedicated file the user reviews
  and applies or rejects. The agent never applies them itself.
- **Watchlist tiers (decided 2026-10-21, applied):** one field, `tracking: active | passive | off`,
  replacing `status` and `priority` in `tracked_stocks.json`. `active` — full research and analysis
  daily, with an announcement check. `passive` — held but not actively traded; mentioned whenever
  movement, unusual volume, or relevant news is detected. `off` — files kept, never selected. The
  morning run checks passive stocks' previous session; **the midday run should check their first
  half of the day** the same way.
- **Longer-cycle runs, to design later, in this order:**
  1. **Weekly (weekend):** reflection — score the week, compare self-directed with scripted calls,
     how views aged, curate lessons. Research — next week's calendar (data releases, policy
     meetings, earnings dates → `upcoming_events`), profiles due for behavior refresh
     (`[research]` → `[observed]`), sector sweep (inactive / candidate), remove past events.
  2. **Monthly (first weekend):** valuation re-check for all active stocks; full baseline research
     for newly added stocks; prune `learned research lessons`; review the month's proposed edits;
     suggest watchlist changes for the user to decide.
  3. **Earnings season (Apr / Aug / Oct):** a wave of financial-snapshot refreshes as reports land.

  **Do a manual monthly run early**, to watch the self-improvement process — it must log what it
  read, what it concluded, and every change it proposed or made, with reasons.

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
  worth the duplication, or is opening each cover page weekly cheap enough? `morning_research.md`
  Phase 3 criterion 6 (stale on time) currently selects them daily — move it if time checks go
  weekly.
- **Overnight flags for abnormal price/volume.** An end-of-day run flags unusual moves for the next
  morning via `open_question` in `daily market notes`; Phase 0 already reads those. Agreed in
  principle (2026-10-15); needs the end-of-day run designed.
- **Marking sectors inactive** (`sectors_readme.md` → *Inactive sectors*): which run checks for
  sectors that have gone cold?
- **`upcoming_events` cleanup**: `tracked_stocks_readme.md` says to remove past events, but no run
  is assigned to do it.

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
- Prediction horizons must match the data available. **Before the skill exists**, the agent relies
  on web search, where intraday figures are hard to find after the fact — so `morning_session`
  predictions may only be scoreable by a midday run. **With the skill**, intraday data is available
  (AKShare offers minute-level bars): it can produce the open, high and low with their times, the
  11:30 level, and the close, which the agent turns into a short narrative. The post-close run can
  then score morning-session calls itself; the midday run stays useful for the afternoon adjustment
  and report.
- The same skill should do the **full-watchlist announcement check**: one market-wide fetch of the
  day's announcements (cninfo / 东方财富), filtered by watchlist codes. Until then,
  `morning_research.md` Phase 3 checks `priority: high` stocks only.

### 14. A free-form thinking step in each program

The user wants the agent's own thinking used outside the fixed structure, not only within it.
`research_playbook.md` → *Self Thinking* allows it in general terms, with a `[self-directed]` tag,
but no program gives it a dedicated step. To propose: one explicit free-form step per program,
how its output is recorded, and how `weekly_reflection.md` compares it with the scripted output.

Decided (user, 2026-10-19):

- **Budget:** up to 3 extra searches per run. A ceiling, not a target — zero is fine when nothing
  off-script is worth following, and the step then says so.
- **Visibility:** the full step goes in `run notes`; its conclusions appear in the Feishu report,
  tagged `[self-directed]`.

- **Output:** may produce both logged predictions (`self_directed: true`) and views. Deliberately
  loose to start — see what it does without a strict program, and restrain it only if results are
  poor.
- **Frequency:** only when the agent judges something is worth following (expected to be fairly
  often). Otherwise one line: "nothing off-script today".

Draft placement: research after Phase 4 (stock profile check); analysis after Phase 4 (stocks)
as "argue against your top call"; a section in `weekly_reflection.md`.

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
5. Check that every rule in a readme that the agent must *act* on has a program step that triggers
   it. A readme says how a file works; a program says when to do something. A readme rule with no
   triggering step is dead text.
6. Show the user each file's diff before moving on to the next.

**Order:** the files the morning run reads first — `stock_profiles_readme.md`,
`morning_research.md`, `morning_analysis.md`, `logs_readme.md`, `sectors_readme.md`,
`research_playbook.md` — then the rest.

### 16. How a call becomes a prediction or a view — revisit

Raised by an independent review (2026-10-19): `morning_analysis.md` → *Forming calls* asks every call
for a direction, timeframe, conditions, confidence, and invalidation — nearly the same as Phase 5's
test for logging a prediction. So every call qualifies, and `view` notes stay empty. The user feels
patching the test adds complexity over a more basic question: how calls are formed in the first
place.

Options (user, 2026-10-21):

1. **Decide at the moment of analysis, by confidence.** The agent decides whether a call is a
   prediction or a view, then formats it — a strict prediction entry (target, threshold, time,
   invalidation) or a simple view ("this rally feels late").
2. **A test applied afterwards** — e.g. "can it be scored mechanically from data that will exist?"

**Caveat on option 1:** confidence and scoreability are different things. The prediction log's
`confidence` field exists so the reflection can check **calibration** — are 0.55 calls right about
55% of the time? If only high-confidence calls are logged, low-confidence ones are never scored,
and the system cannot learn whether its confidence means anything. A precise call held with low
confidence is still worth logging. A hybrid: decide by whether the call *can* be stated precisely;
confidence is then recorded, not used to choose.

**Decided (2026-10-22): the hybrid, applied** in `morning_analysis.md` → *Prediction or view*. The
agent judges by precision, with guidance and examples; the market call and each sector call are
always predictions unless explicitly abstained. The boundary is the agent's judgment — no mechanism.
Later, `weekly_reflection.md` could read the week's views and note which could have been
predictions; add that when the weekly run is designed, not before.

**Knock-on drift:** `research_playbook.md` → *Taking a stance* still says every stance carries a
confidence and what would make it wrong. Views no longer need those. Align it during the clarity
rewrite (Item 15).
