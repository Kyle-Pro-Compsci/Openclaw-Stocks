# Handover — Claude Code maintenance session

Written 2026-10-22, for whoever picks up this repo next (a new Claude Code session, possibly on a
different workstation). This is a maintainer document: it is **not** a runtime instruction file for
the OpenClaw agents. Read `CLAUDE.md` first for the architecture; this file covers where the work
stands and how the user likes to work.

## How to start

1. Read `CLAUDE.md` (architecture, layering rule, conventions, unverified OpenClaw behavior).
2. Read this file in full.
3. Read `workspace-researcher/research_design.md` → **Considered features** and **Open Review
   Items** — the running list of decisions and deferred questions. Most decisions in this session
   are recorded there with dates.
4. Check `git status` / `git log` — the user may have committed since this was written.

## How the user likes to work

These were saved as Claude memory on the old workstation; memory does not travel with the repo, so
they are copied here. Re-save them as memory on the new machine if you can.

- **Plain wording.** The previous maintainer session (Opus 5.0) wrote in compressed, aphoristic
  style the user found hard to follow. Write repo documents plainly, with an example where it helps.
  When reviewing, quote any unclear line and propose a concrete rewrite.
- **Always gloss numbered references.** Never write a bare "Item 7" or "R3" — add what it is in
  brackets, every time: "Item 7 (stop tracking agent-written data in git)".
- **Keep the user able to follow the whole system's logic.** Rewrite section by section, not line
  by line, and never change logic significantly without flagging it first.
- **The user reviews every edit** (manual permission mode). Make edits in readable, focused pieces.
- **`CLAUDE.md`, `claudetodo.txt`, and this file are tracked in git on purpose** during setup, even
  though they deploy into `~/.openclaw/` where the agent could read them. Don't push deployment
  changes unless the user raises it.
- **Independent review agents** have been useful: the user has approved spawning a subagent with
  the full context (goals, files, OpenClaw facts) to critique a design and trace a hypothetical run.

## Where the work stands

Goal: the first supervised manual morning run (see `CLAUDE.md` → *First-run checklist*).

The user has been working through their review notes in `claudetodo.txt`, file by file:

- `stock_profiles_readme.md` — **done** (except the later clarity rewrite).
- `morning_research.md` — **done**. Major changes this session:
  - writes deferred to a single Phase 5 (*Decide and write*); Phases 1–4 note write candidates only;
  - a per-run working file, `run notes` (`shared_files/runs/<date>_morning.md`), protects findings,
    the handoff, and read-check tokens against compaction;
  - two reports: a research report (news digest, no calls) at the end of research, then an analysis
    report;
  - `tracking: active | passive | off` replaces `status` / `priority` in `tracked_stocks.json`;
  - the run covers everything up to the start of today's session; the Asian opens are checked at
    the start of the analysis.
- `morning_analysis.md` — **rewritten this session.** *Live check* (KOSPI/Nikkei opens) → Phase 0
  *Prior positions* → Phase 1 *Assemble* (established / inferred / prior) → Phases 2–4 market →
  sectors → stocks, each level agreeing with the one above → *For every call: why* → *Prediction or
  view* (decided by precision; market and sector calls always predictions) → Phase 5 log → Phase 6
  report.

### Independent review findings R1–R15 (2026-10-19) — status

A subagent traced a hypothetical morning run and found these. Discussed with the user in order.

| # | Finding | Status |
|---|---|---|
| R1 | Every call would land in `prediction log` | **Done** — the hybrid *Prediction or view* rule (Item 16) |
| R2 | "Prior positions" read after research overwrote them | **Done** — research records before-update views in `run notes` |
| R3 | Quiet-stock reads lacked data | **Done** — `tracking` tiers; every active stock is researched daily |
| R4 | Wording assumed the market had already traded | **Done** — "up to the start of today's session" rule; Asian opens moved to analysis |
| R5 | Compaction can erase the program's own steps and read-check tokens | **Done** — re-read each phase's section; tokens written to `run notes` |
| R6 | Two reports conflict with `AGENTS.md` ("consolidated report"; "don't paste full reports in chat"); mid-run sending unverified | **Next to discuss** — proposed fix: update `AGENTS.md`, and a fallback if mid-run sending fails |
| R7 | First run stalls on empty files (`TODO` sectors, no profiles) | Pending — proposed: fill `TODO`s when established; create thin profiles for active stocks |
| R8 | Analysis rules contradict on re-opening files | Largely solved by R3 — confirm |
| R9 | Item 9 (log timezone/IDs) must be fixed before the first run; nothing writes `market outcomes` yet | Pending — proposed: do Item 9 next; minimal post-close scorer early in the timing work |
| R10 | Unverified whether "append" exists as a tool action | Pending — add to `CLAUDE.md` first-run checklist |
| R11 | Stale reference in `morning_analysis.md` → *Where the rules live*: "(Phase 3)" should be "(Phase 5)" | Pending, trivial |
| R12 | Research Phase 0 asks questions answered by files it doesn't read yet | Pending — move them to Phases 2 and 3 |
| R13 | A sector's first sighting is usually dropped, so "comes up twice" never triggers | Pending — `candidate sector:` prefix, exempt from dropping |
| R14 | Nothing triggers sectors-readme route 2 (watchlist sectors); watchlist sector names don't match `sectors.md` names | Pending |
| R15 | Label vocabularies are copied into research Phase 2 and have drifted (`unsure`) | Pending — keep the lists only in the readme |

### Suggested order from here

1. Finish R6–R15 with the user.
2. **Item 9** in `research_design.md` (log timestamps vs. the Shanghai trading day, ID scheme,
   outcome verdict) — before the first run, since the logs are append-only.
3. **Item 12** (run schedule) — the post-close run first, since the morning run depends on it
   (`recent_activity`, a daily summary note, `market outcomes`, abnormal-move flags).
4. **Item 14** (free-form thinking step) — decisions already made, needs writing into programs.
5. Item 8 (drift cleanup) and Item 15 (full clarity rewrite).
6. The first supervised manual run; time it (08:00 start, report due by 09:00).

## Facts established this session

- **OpenClaw** (docs.openclaw.ai): skills are `SKILL.md` instructions plus optional scripts run via
  the `exec` tool. Auto-compaction is on by default; pruning of old tool results is off for
  non-Anthropic providers. Details and what is still unverified are in `CLAUDE.md` → *Unverified*.
- **Model:** the user plans to use Moonshot Kimi (K2.6 has a ~256K context window). Pricing is
  low enough that results matter more than cost, but write efficiently.
- **Market timing (Shanghai):** KOSPI and Nikkei open 08:00; A-shares auction 09:15, open 09:30;
  Hong Kong opens 09:30; lunch break 11:30–13:00; close 15:00.
