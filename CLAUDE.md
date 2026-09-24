# CLAUDE.md — OpenClaw Stocks

Multi-agent China-market (A-share) research system built on OpenClaw. Goal: automated daily
news/market research feeding sector and stock predictions, with a self-improvement loop that
scores predictions against outcomes.

## What this repo is

This repo holds the editable parts of an OpenClaw install whose live root is `~/.openclaw/`.
Nothing executes from this checkout — it is the authoring copy. **Deployment is a `git pull` on the
OpenClaw workstation**, where the repo root sits at `~/.openclaw/`. See *Deployment* below,
including the caveat that the agent writes to git-tracked files.

`.gitignore` allows only `workspace/`, `workspace-researcher/`, `shared_files/`, and
`.clinerules.md` — the live root also holds `reference/` (the OpenClaw doc mirror), `openclaw.json`,
and credentials, which must never be committed. That is why those files are not visible here: do not
assume runtime behavior that can only be confirmed in them — see *Unverified* below.

## The two agents

| Agent | Workspace | Role |
|---|---|---|
| Supervisor | `workspace/` | Coordinator, Feishu-facing main agent |
| Researcher | `workspace-researcher/` | Macro, sector, and stock research |

Shared state lives in `shared_files/` and is addressed by **key** through
[shared_files/paths.json](shared_files/paths.json) — e.g. `tracked stocks`, `source guide`,
`logs readme` — never by raw path. If a file is not in the workspace root, resolve it through
`paths.json` before concluding it does not exist.

## The layering rule

A rule lives in exactly one layer. Everywhere else, link to it.

1. **Always-loaded instructions** — `AGENTS.md` (per workspace)
2. **General methodology** — `workspace-researcher/research_playbook.md`
3. **Executable programs** — `workspace-researcher/programs/*.md`
4. **Readmes that own a data file's schema** — `*_readme.md`
5. **Data files** — `tracked_stocks.json`, `sectors.md`, stock profiles
6. **Append-only logs** — `shared_files/logs/*.jsonl`
7. **Generated caches** — `shared_files/stock_profiles/<code>/`

### The placement test

When deciding where a rule belongs:

- Would it still matter if the agent were answering a **one-off question**? → constraint → `AGENTS.md`
- Only relevant when **researching**? → methodology → `research_playbook.md`
- Only during **one scheduled run**? → that program file
- Describes a **field or file layout**? → the owning readme

Worked example — prediction rules, formerly duplicated across six files:

| Layer | File | Holds |
|---|---|---|
| Constraint | `AGENTS.md` | "Only log falsifiable predictions." One line. |
| Methodology | `research_playbook.md` | How to form a good one; when to decline to predict. |
| Schema | `logs/logs_readme.md` | The exact field list. Single source of truth. |

## User vs AI authorship

Keeping these separate is a core design goal. Never merge them.

- **`user_research_priorities.md`** — human-authored. The agent writes here only when the user
  gives an explicit standing instruction ("watch gold prices").
- **`learned_research_lessons.md`** — agent-curated durable lessons. Self-derived learning goes
  here, never into the priorities file. Read every morning; weighted by recency and by the market
  phase each lesson was learned in. Its header documents its own format — there is no separate
  readme, because the read-weighting rules must travel with the content.
- **`logs/weekly_reflections.jsonl`** — raw agent reflection output. Lessons graduate from here
  into `learned_research_lessons.md`; raw reflections never get dumped into the playbook or
  `AGENTS.md`.

## File responsibility map

| Rule type | Canonical home |
|---|---|
| Agent identity, hard constraints, shared-file protocol | `workspace-researcher/AGENTS.md` |
| General research methodology | `workspace-researcher/research_playbook.md` |
| Daily morning research execution order | `workspace-researcher/programs/morning_research.md` |
| Daily morning view, predictions, and report | `workspace-researcher/programs/morning_analysis.md` |
| Weekly baseline research run (outline only) | `workspace-researcher/programs/weekly_research.md` |
| End-of-day outcome review | `workspace-researcher/programs/daily_reflection.md` |
| Weekly self-improvement / lesson curation | `workspace-researcher/programs/weekly_reflection.md` |
| Source quality and credibility | `shared_files/source_guide.md` |
| JSONL log schemas | `shared_files/logs/logs_readme.md` |
| `sectors.md` maintenance | `shared_files/sectors/sectors_readme.md` |
| Watchlist schema | `shared_files/tracked_stocks/tracked_stocks_readme.md` |
| Stock profile rules | `shared_files/stock_profiles/stock_profiles_readme.md` |
| Human priorities | `shared_files/user_research_priorities.md` |
| Agent lessons | `shared_files/learned_research_lessons.md` |
| Design scratchpad (non-runtime) | `workspace-researcher/research_design.md` |

`workspace-researcher/_examples/` holds AI-generated drafts kept for reference only. They are
**not** in the agent's read path and must not be cited as rules.

### Where deferred design notes live

**`workspace-researcher/research_design.md` → "Open Review Items"** holds design questions and
review tasks addressed to *whoever maintains this repo* — a Claude Code session or the user — not
to the OpenClaw researcher agent. Check it at the start of a maintenance session and add to it
whenever a design question is raised but deliberately deferred.

That file serves two audiences safely because the researcher agent is instructed to treat it as a
non-operational scratchpad and ignore it during normal runs. Keep it that way: nothing in it
should read as a runtime instruction.

Currently open there: (1) audit whether the readme-before-**write** rule holds for every data
file, since some readmes carry interpretation rules needed for *reading*; (2) a full end-to-end
walkthrough of a hypothetical morning run, reasoning as OpenClaw would actually execute it rather
than as the documents intend.

## Read-check tokens (temporary bring-up scaffolding)

Guidance files carry a `<!-- READ-CHECK: RC-… -->` token. The researcher is instructed
(`research_playbook.md` → *Read-check tokens*) to list every token it encountered at the end of a
run. This verifies which files were **actually loaded** across a multi-step cron job — a list of
filenames can be reconstructed from `paths.json` without opening anything, but a random token
cannot.

> **⚠ Open issue — the registry below leaks to the agent.** With `!CLAUDE.md` in `.gitignore`, this
> file syncs into `~/.openclaw/` along with everything else, so the researcher can read this table
> and could echo tokens for files it never opened — which defeats the mechanism entirely. Until
> that is resolved, treat token reports as *weak* evidence. Fixes: exclude `CLAUDE.md` from the
> deploy step, or move this table to a gitignored local file. Owner: user.

| Token | File | Placement |
|---|---|---|
| `RC-AGENTS-R4W9` | `workspace-researcher/AGENTS.md` | bottom |
| `RC-PLAYBOOK-K7X2` | `workspace-researcher/research_playbook.md` | bottom |
| `RC-MORNING-N4T7` | `workspace-researcher/programs/morning_research.md` | bottom |
| `RC-ANALYSIS-V7H2` | `workspace-researcher/programs/morning_analysis.md` | bottom |
| `RC-SOURCE-M4Q9` | `shared_files/source_guide.md` | bottom |
| `RC-LOGS-H9V4` | `shared_files/logs/logs_readme.md` | bottom |
| `RC-TRACKED-Z3D7` | `shared_files/tracked_stocks/tracked_stocks_readme.md` | bottom |
| `RC-PROFILES-R6L8` | `shared_files/stock_profiles/stock_profiles_readme.md` | bottom |
| `RC-PRIOR-F8K2` | `shared_files/user_research_priorities.md` | bottom |
| `RC-SECRDME-TOP-T8B3` | `shared_files/sectors/sectors_readme.md` | **top** |
| `RC-SECRDME-END-W5J1` | `shared_files/sectors/sectors_readme.md` | **bottom** |
| `RC-SECTORS-P2N6` | `shared_files/sectors/sectors.md` | header |
| `RC-LESSONS-C1Y5` | `shared_files/learned_research_lessons.md` | end of protected header |

**How to read the results:**

- Token present → that file was loaded this run.
- Token absent → probably not loaded. Weak evidence alone (the agent may read and forget to
  report), but consistent absence across runs is a real finding.
- `sectors_readme.md` is the largest guidance file at 9KB and carries a **pair**. TOP without END
  means a truncated or partial read — the most useful single signal here.
- `paths.json` deliberately has no token: adding a key would pollute a schema the agent iterates
  over, and any correctly-resolved shared path is already evidence it was read.

**Limitation:** a token proves a file was *loaded*, not that its rules were *followed*.

**Removal:** once the chain is trusted, find them with
`grep -rl "READ-CHECK" workspace-researcher shared_files` and delete the matching lines, then
remove the *Read-check tokens* section from `research_playbook.md` and this section.

## Conventions

- **Logs are append-only.** Never rewrite an entry. Corrections are new entries; outcomes link
  back via `linked_prediction_id`.
- **`*_readme.md` files are read-only** to the agent unless the user asks for a change.
- **Readme before write, not before read.** A readme is required before *writing* to its file, not
  merely to read it. Deferring a readme read within a run saves nothing (context is cumulative);
  the only saving comes from not reading it at all when no write happens. Read *ordering* is a
  program concern — put it in the program file, not in `AGENTS.md`. See Open Review Item 1 in
  `research_design.md`: some readmes may carry interpretation rules genuinely needed for reading.
- **Profiles update section-by-section.** Never regenerate a whole stock profile to tidy it.
- **No fabricated data.** A blocked source, a failed search, or a missing file gets reported as
  such. Never fill a gap with something that sounds right.
- **Kimi/Moonshot search is a discovery layer**, not a source. Treat a synthesized answer as a
  lead until source name, date, and link are identified.
- **Stock-specific detail never enters `sectors.md`.** A stock's role within its sector belongs in
  that stock's profile.

## Current status

Scaffolded, not yet run. The immediate goal is the first end-to-end morning research run.

- **Working toward first run:** `programs/morning_research.md` + `programs/morning_analysis.md` —
  the pre-open pair. Research gathers, analysis concludes; one cron runs both in sequence.
- **Written, not yet exercised:** `programs/weekly_reflection.md` (weekly review + the lesson
  curation rules for `learned_research_lessons.md`)
- **Outline only:** `programs/weekly_research.md` (weekly baseline run — marked WIP, has open
  questions about where the baseline is stored)
- **Stubbed:** `programs/daily_reflection.md` (4-line purpose note)
- **Empty by design:** all four `logs/*.jsonl` (append-only, nothing logged yet)
- **Needs user input:** `tracked_stocks.json` is valid but empty `{}` — no watchlist chosen yet.
  Until it has entries, the morning run's stock phases are no-ops and only the macro/sector half
  does work.
- **Seeded skeletons:** `sectors.md` has entries whose fields are `TODO` pending the first run.

## The morning cron job

**Not yet created** — do a supervised manual run first (see below).

The prompt is deliberately thin. All methodology lives in the files; a fat cron prompt would become
a fourth place where rules live and drift out of sync.

> Run the morning research program. Read `programs/morning_research.md` in your workspace and
> execute it in order, then read `programs/morning_analysis.md` and execute that. Resolve all shared
> files through `~/.openclaw/shared_files/paths.json`. Do not skip phases. If a required file is
> missing or a source is blocked, say so explicitly rather than filling the gap. End with the single
> consolidated report from the analysis program, followed by the read-check token list.

**Both files in one prompt, deliberately.** They must share a session — the analysis run works from
the research findings held in context. Splitting them into two cron jobs would force the analysis to
reconstruct state from the logs, which is both lossy and expensive.

**Timing:** pre-open, Asia/Shanghai (GMT+8). Roughly 07:30–08:30, finishing before the 09:15
auction. Leave enough margin that a slow search does not push the report past the open.

## Deployment

Deployment is a `git pull` on the OpenClaw workstation, where this repo's root sits at
`~/.openclaw/`. That is also why `.gitignore` excludes everything except `workspace/`,
`workspace-researcher/`, `shared_files/`, and `.clinerules.md` — the live root holds
`reference/`, `openclaw.json`, and credentials that must never be committed.

### ⚠ The agent writes to git-tracked files

This is the thing most likely to bite. The researcher writes to `shared_files/` during normal
operation — `logs/*.jsonl`, `sectors.md`, and `stock_profiles/<code>/`. All of those are tracked.

Consequences to watch for:

- **A pull can conflict with agent-written data.** After the system has run, the deploy machine has
  local modifications. `git pull` may refuse or conflict — most likely on the append-only logs,
  where both sides have appended different lines. Commit or stash the agent's output before pulling.
- **Agent output is invisible here until committed and pushed** from the deploy machine. Reviewing a
  run's logs or reflections from this PC requires that round trip.
- **Never resolve a log conflict by taking one side.** They are append-only; both sets of lines are
  real. Keep both and keep them in timestamp order.
- Worth deciding early whether agent-written data should be tracked at all, or moved to a gitignored
  path with only the schemas and seeds in git. Tracking it gives history and cross-machine review;
  not tracking it removes this whole class of conflict.

### First-run checklist

1. **Pull** onto the OpenClaw workstation.
2. **Check paths resolve** — confirm `~` expands as expected on that machine and that a few
   `paths.json` keys point at files that actually exist.
3. **Consider hiding `CLAUDE.md`** from the agent — it carries the read-check token registry, which
   the agent could echo without opening anything (see the warning above).
4. **Add stocks** to `tracked_stocks.json`, which is `{}` today. Until it has entries the stock
   phases do nothing and only the macro/sector half of the run does work. Schema is in
   `tracked_stocks_readme.md`.
5. **Run manually once** with the cron prompt above, and check:
   - every `paths.json` key resolved, and nothing was read that no phase called for;
   - appended JSONL validates against `logs_readme.md`;
   - any new `sectors.md` entry matches the readme's format, with no stock-specific detail in it;
   - the report separates facts / interpretation / prediction / uncertainty, and actually commits to
     a view rather than only describing;
   - the read-check list shows the files you expected — especially **both** `RC-SECRDME-TOP-T8B3`
     and `RC-SECRDME-END-W5J1`, since TOP without END means a truncated read.
6. **Then** create the cron job, once the agent name/id below is known.

## Unverified OpenClaw behavior

Not confirmable without the doc mirror or the live install. Do not assume:

- Whether `standing_orders.md`-style files are auto-injected each session (the archived file
  claimed it; constraints were moved into `AGENTS.md` so nothing depends on the claim).
- The researcher agent's configured name/id, needed to target a cron job.
- **What file-read primitives the agent has** — whether it can read part of a file (offset/limit),
  run shell commands (`tail -n 20`), or only read whole files. Load-bearing: every append-only log
  grows forever, and `morning_research.md` Phase 0 reads "recent" entries from several of them every
  single day. If whole-file reads are the only primitive, the daily run's cost grows without bound.
  Current approach is deliberately simple — one uncapped file per log, with an instruction to weight
  recent entries — which addresses *attention* but not *context cost*. Revisit once a log actually
  gets long; options are rotation or time-partitioned files (`daily_market_notes_2026-09.jsonl`).
- Whether a cron run counts as a "main session" and therefore receives `MEMORY.md`. Assumed **no**
  — which is why research lessons live in `shared_files/learned_research_lessons.md` (reachable
  from any session) rather than in `MEMORY.md` (main sessions only). If this assumption is wrong,
  the split is still correct, but the reasoning behind it changes. Verify before relying on it.
- Cron syntax, delivery target, and whether a cron run gets a fresh session.
- Whether the runtime expands `~` on Windows.
