# Morning Research Run

## Purpose
The daily morning run collects important developments since the last run, detects what changed, and updates cached research only when needed. It runs pre-open, and hands its findings to `morning_analysis.md` for the directional view.

The process starts wide and narrows, going from:

Global / macro -> Chinese market -> Sector and theme movement -> Tracked Stocks

This program should avoid generic market commentary. It should answer:

1. What changed since the last relevant scan?
2. Which geopolitical, macro, policy, sector, or company developments matter today?
3. Which tracked sectors/stocks are affected?
4. Does the new information change any sector view, stock profile, financial view, or monitoring rule?
5. What needs watching during the trading day?

## Scope: research, not analysis

This program **gathers and records**. It establishes what happened and what changed.

It does **not** form the directional view for the day. That is `morning_analysis.md`, which runs
immediately after this program in the same session, with this run's findings still in context.

The dividing line is the same one used elsewhere in this system: **settled vs. pending.** What has
already happened is research. What will happen is analysis.

So this program writes observations — `daily market notes` with `kind: observation`, sector updates,
profile updates. It does **not** write to `prediction log` or write `view` notes; those belong to the
analysis run.

**This program covers everything up to the start of today's session.** The A-share market has not
opened yet. "Last session" means the previous trading day. For any market that has not opened yet,
report its last session and say so — never report a value for a market that is not yet trading.
The Asian opens (KOSPI, Nikkei, both 08:00 Shanghai time) are checked later, at the start of
`morning_analysis.md`. Hong Kong opens at 09:30, after both reports.

## Output

This program sends one message: the **research report**, at the end of Phase 6. Do not send a
message per phase. `morning_analysis.md` then sends a second report with the day's view.

Exception: when a human is running this interactively and asks to see the work, per-phase output is
fine.

## Where the rules live

This program sets the order of work only. The rules for writing to a file live in that file's
readme — read it before writing (see `AGENTS.md`). Research methodology is in
`research_playbook.md`.

## The run notes file

Every run keeps one working file, `run notes` (resolve through `paths.json`), named
`<YYYY-MM-DD>_morning.md` with the Shanghai date. It is not a log, and nothing reads it except
today's run.

It exists because a long run can be **compacted**: older parts of the session are summarised to
save space, and exact figures and sources can be lost in the summary. The file keeps them exact.

- Create it at the start of Phase 0. If today's file already exists (a rerun), add a
  `# Rerun <HH:MM>` heading and continue below it — do not overwrite.
- At the end of Phases 1–4, append that phase's section: the key findings the analysis will need,
  and each **write candidate** — what was found, the source name, date, and link, and where you
  think it belongs.
- Copy figures exactly as the source states them. Do not round or paraphrase.
- **Write nothing else before Phase 5.** Phases 1–4 only note candidates.
- When you need an earlier finding, read it from this file rather than relying on memory. If memory
  and the file disagree, the file wins.
- **At the end of each phase**, write `Next: Phase N` in this file, then re-read that phase's section
  of this program before starting it. Compaction can summarise the program's own instructions away,
  not only the findings.
- **When you see a `READ-CHECK` token** in any file, record it here straight away, under
  `## Read-checks`. The final token list is built from this section, not from memory.

Format:

```markdown
# Morning run 2026-10-15

## Phase 1 — Macro
### Findings
- Brent +5.2% overnight on reports of a tanker seizure near Hormuz (Reuters, 2026-10-15 03:10 CST, <link>)
### Write candidates
- [daily market notes] Brent +5.2% on Hormuz tanker reports — Reuters, 2026-10-15, <link>
```

## Phase 0: Setup and recent-state check

Create today's `run notes` file (see *The run notes file* above).

Read `learned research lessons` and `user research priorities`.

Both shape what the rest of the run looks for, so they come first. Lessons are weighted by recency
and by the market phase they were learned in — that file's header explains how to apply them.

Then check recent log entries — enough to answer these, not exhaustively:

* What was already known yesterday or in the last run?
* Are there open predictions needing follow-up? (`prediction log` — check `what_to_check_later`)
* Are there unresolved `open_question` entries in recent `daily market notes` to pick up today?
* Which sectors/themes are already marked hot, warming, cooling, or uncertain?
* Which tracked stocks are `active`, and which `passive`?

Read recent entries only. These logs grow indefinitely and old entries are rarely relevant today.

## Phase 1: Wide macro / geopolitical / economic scan

Start with broad context before looking at individual stocks.

Check for current developments in:

* Major geopolitical events
* U.S. market movement and overnight global risk sentiment
* U.S. rates, dollar, commodities, oil, gold, and major index moves
* China macro, policy, liquidity, regulators, official economic data
* Hong Kong market and China ADR spillover when relevant
* User-priority themes, from the `user research priorities` read in Phase 0
* Any urgent event likely to affect A-share open, sector rotation, risk appetite, or policy expectations
* At least one other topic that you judge to be relevant. Use your own judgement.

Output of this phase should be a concise “morning context” summary:

* what changed;
* why it matters;
* affected markets;
* affected sectors/themes;
* confidence;
* source notes;
* uncertainties.

Do not turn weak rumors into facts. Label rumors, blocked sources, and source gaps clearly.

Append this summary, and a write candidate for each key development, to `run notes`.

## Phase 2: A-share market regime and sector/theme scan

Read `sectors`. Use the broad context to assess today's likely A-share setup.

Check:

* **Market regime** — what kind of market today is: risk-on, risk-off, policy-driven, liquidity-driven, theme-driven, consolidation/震荡, rebound, breakdown risk, etc.
* Major index relevance: 上证指数, 深证成指, 创业板, 科创50, 北证 if relevant.
* Sector and theme heat.
* Whether current hot sectors overlap with tracked industries/stocks.
* Whether current events confirm or contradict existing sector views.
* Whether any sector/theme deserves follow-up chained searches.

### Per sector: is it already tracked?

**If the sector already has an entry in `sectors.md`:**

1. Read its current entry.
2. Do not trust the existing label mechanically. Ask the review questions from `sectors readme`:
   is the current view still accurate; does new information support, weaken, or complicate it; is
   the sector getting hotter, colder, broader, narrower, more speculative, or more fundamentally
   supported; does it deserve more, less, or the same attention today?
3. If the answers change the sector-level view → write candidate for `sectors`.
4. If today's information is real but does not change the sector view → write candidate for
   `daily market notes`.
5. If nothing changed → no candidate. An unchanged sector needs no write.

**If a sector is not in `sectors.md` but comes up today:** note it as a write candidate for a new
sector entry, with the reason it came up. Phase 5 decides whether it qualifies, using
`sectors readme` → *Adding a new sector*. Research it today only as far as the rest of the run
needs.

### Classification

For each important sector/theme, classify using the vocabularies in `sectors readme`. If no label
fits well, pick the closest and explain the difference in one line.

* `heat_status`: ignored / warming / hot / cooling / exhausted / reactivating / unclear
* `stage`: early / middle / late / post-hype / unclear
* `driver`: policy / macro / earnings / overseas spillover / liquidity / geopolitics / commodity price / supply-chain event / theme speculation / company announcements / unclear
* `move_quality`: broad / leader-led / follower-led / speculative / weak / mixed / unclear
* `view_change`: no change / supported / weakened / changed / unclear — **and why**, in one line
* `follow_up_needed`: yes/no and why

If `heat_status` differs from the current entry, say why in one line.

`view_change` and `follow_up_needed` are working labels for this run — they go in `run notes`, not in
`sectors.md`.

**Do not record which tracked stocks belong to a sector in `sectors.md`.** Stock-specific sector
role belongs in that stock's profile. Note the overlap in `run notes` for Phase 3 instead.

Append this phase's findings, classifications, and write candidates to `run notes`. For each sector
reviewed, also record its current view **as it stands before any update today** (one line: current
view, heat status, last reviewed). `morning_analysis.md` uses these as its prior positions.

## Phase 3: Tracked stock selection

Read `tracked stocks`.

Each stock's `tracking` value decides how it is handled (see `tracked stocks readme`):

* `active` — always selected, and always gets the announcement check below.
* `passive` — selected only if it matches one of criteria 2–6 below. Check each passive stock's
  previous session for an unusual move or volume: use the post-close run's `open_question` flags,
  and if those are missing, a quick check where data allows. Anything unusual selects it
  (criterion 3), and it gets a mention in both reports.
* `off` — never selected.

**Announcement check.** For each `active` stock, search for company announcements since the last run
(巨潮资讯 / cninfo is the primary source). For `passive` stocks, rely on news already found in
Phases 1–2. If you find a dated future event — an earnings date, a 解禁 (lock-up expiry), a
shareholder meeting — note a write candidate for that stock's `upcoming_events`.

Select stocks for review when they match one or more of:

1. `tracking: active`.
2. Stocks whose sector, industry, or tags were touched by Phase 1 or Phase 2.
3. Stocks with direct news, announcements, or abnormal price/volume behavior, or with an
   `upcoming_events` date today or in the next two trading days.
4. Stocks connected to active user research priorities.
5. Stocks with open predictions or `open_question` notes requiring follow-up.
6. Stocks whose cached profile is stale on time (see `stock profiles readme`).

For each selected stock, note in `run notes` which criterion selected it, in one line.

**Selection matters more than it looks: a stock that is not selected is not checked at all**, no
matter what happened to it. Criterion 2 is what prevents that, and it works from
`tracked_stocks.json` alone (which carries sector, industry, and tags) without opening any profile.

Do not analyse every stock in full when nothing changed. For a checked but unaffected stock, "no
material new information found" is a complete and acceptable result.

## Phase 4: Stock profile/cache check

Each stock's cache is three files in `shared_files/stock_profiles/<bare 6-digit code>/`. See
`stock profiles readme` for the layout, folder naming, and staleness rules.

For each selected stock:

1. Check whether the stock's folder exists.
2. **If it does not**, this stock has never been researched. Creating a baseline is a research task
   in its own right — do not attempt a full one mid-run. Note a write candidate for a new profile
   built from what today's research established, with the gaps listed for
   `sections_needing_refresh`; or flag it for a dedicated baseline run. Mark a thin profile clearly
   as incomplete, so it is not later relied on as a full one.
3. **If it does**, read `stock_profile.json` first. It is the cover page, and it is deliberately
   short. Its `recent_activity` lists the last few notable trading days, and its `financial_digest`
   carries the current valuation verdict and its date.
4. Use it to decide whether either heavier file is needed. Usually neither is.

**Open `financial_valuation_snapshot.json` only when:**

* financial quality, earnings, revenue, margins, cash flow, or balance sheet matters;
* new information claims the business is improving or deteriorating;
* the narrative depends on whether the company is a real beneficiary or only a proxy/concept stock;
* valuation, price level, rerating, or whether a move is justified matters;
* the price has moved sharply.

**Open `trading_history.jsonl` only when:**

* a price level is in play — a prior high, low, or pressure point;
* you need to know whether a move like today's has stuck before;
* the stock's behavior looks out of character and the profile's personality verdict needs checking
  against the record.

Weight recent entries well above older ones. Do not use the history to infer what is *normal* for
the stock — every entry in it is by definition an abnormal day.

The profile's `news_filter` decides whether today's news is worth reacting to at all. Check it
before doing deeper work on a stock.

Append each stock's findings and write candidates to `run notes`. For a stock with nothing new, one
line saying so is enough. For each stock, also record its narrative **as it stands before any update
today** (one line: `current_narrative.story`, its status, last reviewed). `morning_analysis.md` uses
these as its prior positions.

## Chained follow-up searches

Can trigger during any phase. Rules and the search budget are in `research_playbook.md` →
*Chained Follow-Up Searches*. Do not chase every interesting thread.

## Phase 5: Decide and write

Purpose: write each new, lasting finding **once**, to the right file. Until now nothing has been
written except `run notes`. Logs are append-only, so a duplicate or a premature entry written here
stays forever.

1. **Read `run notes` in full.** Work from it, not from memory.
2. **List the write candidates** from Phases 1–4.
3. **Merge duplicates.** One `daily market notes` entry per event. A sector or profile update records
   a change to a current view, not the event itself, so it can sit alongside that note.
4. **Drop what changes nothing** — the default; on most days, most candidates. For each, ask:
   - Is it new, or was it already known?
   - Does it change a current view — sector heat, driver, move quality, a stock's narrative, news
     filter, financials, valuation, or how it trades?
   - Is it lasting enough for `sectors.md` or a profile, or only a daily observation?
5. **Write the rest.** Read each destination's readme first if you have not already this run.
   Update only the section that changed — never regenerate a file to tidy it. Destinations:
   - `daily market notes` — `kind: observation`. Set `open_question` when the note leaves a specific
     thread a later run could resolve.
   - `sectors` — including new entries, if they qualify under `sectors readme` → *Adding a new
     sector*.
   - stock profiles — including new profiles, `recent_activity`, and the `financial_digest`.
   - `tracked stocks` — `upcoming_events`.
6. **Record in `run notes`** what you wrote, and anything flagged but not written (for example, a
   needed refresh there was no time for).

Do not write a `trading_history.jsonl` entry for today's move: `follow_through` cannot be known on
the day itself. Record the day in the profile's `recent_activity` instead.

**Do not write to `prediction log`, and do not write `view` notes, in this program.** Both belong to
`morning_analysis.md` — this run records what happened, not what will happen.

## Phase 6: Hand off and send the research report

Write the handoff as the final section of `run notes`, under `## Handoff`. `morning_analysis.md`
reads it before forming any view, so it must stand on its own:

1. Morning macro/geopolitical context — what changed and why it matters.
2. Market regime (from Phase 2).
3. Sector/theme findings, with the classifications from Phase 2.
4. Tracked stocks checked, and what was found for each — every `active` stock, plus any `passive`
   stock that was selected today.
5. File updates made, and any flagged as needed but not done.
6. Open questions carried in from Phase 0, and whether today resolved them.
7. Source gaps, blocked sources, and anything that could not be verified.

**Cover what did not change, too.** Every user priority, every sector reviewed, and every stock
checked gets at least one line — "Gold flat (+0.1%), consistent with the current view" or "no
material news for 688776". Steadiness is information: it tells the analysis which views still hold.
What Phase 5 drops is the *file write*, not the finding.

Keep facts separate from interpretation throughout. The analysis run needs to know what is
established versus what is inferred — if that line is blurred here, the stance built on it will
inherit the error.

If nothing material changed, say so plainly, with a short read of the steady picture. A quiet
morning is a finding, not a failure.

Then **send the research report**, built from the handoff. It is the morning news digest for the
trading team: what happened overnight and this morning, what it means for A-shares, what changed
and what held steady, and what could not be verified. It contains no directional calls — those come
in the analysis report.

<!-- READ-CHECK: RC-MORNING-N4T7 -->
