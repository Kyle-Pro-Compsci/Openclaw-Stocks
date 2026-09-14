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

So this program writes observations — `daily market notes`, sector updates, profile updates. It does
**not** write to `prediction log`; that belongs to the analysis run.

## Output

Do not send a message per phase. This program hands its findings to `morning_analysis.md`, which
produces the single consolidated report.

Exception: when a human is running this interactively and asks to see the work, per-phase output is
fine.

## File responsibility

This program controls **flow** — what happens in what order. It does not restate write rules. Each
guide owns its own:

* `sectors readme` — when and how to update `sectors.md`
* `logs readme` — JSONL schemas and append rules
* `stock profiles readme` — profile layout, staleness, and update rules
* `source guide` — source reliability
* `research_playbook.md` — research methodology
* `paths.json` — where everything lives

During the run, collect candidate updates. Write only when the relevant guide says the update
belongs there.

## Phase 0: Setup and recent-state check

Read `learned research lessons` and `user research priorities`.

Both shape what the rest of the run looks for, so they come first. Lessons are weighted by recency
and by the market phase they were learned in — that file's header explains how to apply them.

Then check recent log entries — enough to answer these, not exhaustively:

* What was already known yesterday or in the last run?
* Are there open predictions needing follow-up? (`prediction log` — check `what_to_check_later`)
* Are there unresolved `open_question` entries in recent `daily market notes` to pick up today?
* Which sectors/themes are already marked hot, warming, cooling, or uncertain?
* Which tracked stocks are active or high priority?

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

Record key developments in `daily market notes` (Phase 6 covers the write rules).

## Phase 2: A-share market regime and sector/theme scan

Read `sectors`. Use the broad context to assess today's likely A-share setup.

Check:

* Overall market regime: risk-on, risk-off, policy-driven, liquidity-driven, theme-driven, consolidation/震荡, rebound, breakdown risk, etc.
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
3. If the answers change the sector-level view → candidate `sector_update`.
4. If today's information is real but does not change the sector view → `daily_market_note`.
5. If nothing changed → record nothing. An unchanged sector needs no write.

**If the sector is not in `sectors.md` but matters today:**

1. Judge whether it is genuinely relevant — tied to user priorities, tracked stocks, or a real
   market move — rather than just a passing headline.
2. If it is, research it from scratch and create a new entry.
3. If evidence is thin, note it in `daily market notes` and leave `sectors.md` alone until it earns
   an entry.

Read `sectors readme` before writing anything to `sectors.md` — it owns the entry format and the
controlled vocabularies below.

### Classification

For each important sector/theme, classify using the vocabularies in `sectors readme`:

* `heat_status`: ignored / warming / hot / cooling / exhausted / reactivating / unclear
* `stage`: early / middle / late / post-hype / unclear
* `driver`: policy / macro / earnings / overseas spillover / liquidity / geopolitics / commodity price / supply-chain event / theme speculation / company announcements / unclear
* `move_quality`: broad / leader-led / follower-led / speculative / weak / mixed / unclear
* `view_change`: no change / supported / weakened / changed / unclear
* `follow_up_needed`: yes/no and why

**Do not record which tracked stocks belong to a sector in `sectors.md`.** Stock-specific sector
role belongs in that stock's profile. Note the overlap in your working notes for Phase 3 instead.

## Phase 3: Tracked stock selection

Read `tracked stocks`.

Select stocks for review when they match one or more of:

1. High-priority active stocks.
2. Stocks whose sector, industry, or tags were touched by Phase 1 or Phase 2.
3. Stocks with direct news, announcements, or abnormal price/volume behavior.
4. Stocks connected to active user research priorities.
5. Stocks with open predictions requiring follow-up.
6. Stocks whose cached profile is stale on time (see `stock profiles readme`).

**Selection carries more weight than it appears to.** A stock's refresh triggers can only be checked
once it is selected — a stock never selected is never examined at all, no matter what happened to
it. Criterion 2 is what prevents that, and it works from `tracked_stocks.json` alone (which carries
sector, industry, and tags) without opening any profile.

Do not analyse every stock in full when nothing changed. For a checked but unaffected stock, "no
material new information found" is a complete and acceptable result.

## Phase 4: Stock profile/cache check

Each stock's cache is three files in `shared_files/stock_profiles/<bare 6-digit code>/`. See
`stock profiles readme` for the layout, folder naming, and staleness rules.

For each selected stock:

1. Check whether the stock's folder exists.
2. **If it does not**, this stock has never been researched. Creating a baseline is a research task
   in its own right — do not attempt a full one mid-run. Either create the profile from the
   templates with what today's research established and list the gaps in `sections_needing_refresh`,
   or flag it for a dedicated baseline run. Do not let a thin profile later pass as a researched
   one.
3. **If it does**, read `stock_profile.json` first. It is the cover page, and it is deliberately
   short.
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

## Chained follow-up searches

Can trigger during any phase. Rules and the search budget are in `research_playbook.md` →
*Chained Follow-Up Searches*. Do not chase every interesting thread.

## Phase 5: Change detection and write decisions

For each relevant macro item, sector, or stock, ask:

1. Is this new, or was it already known?
2. Does it change the current view?
3. Does it change sector heat, driver, move quality, the stock narrative, the news filter, financial
   quality, valuation assumptions, or how the stock trades?
4. Is the evidence strong enough to preserve in a baseline file, or is it only a daily observation?
5. Does it confirm an existing view without needing an update?
6. Does it contradict one, requiring correction?

Classify each candidate update:

* `no_write` — the default. Most days most things have not changed.
* `daily_market_note`
* `sector_update`
* `stock_profile_update`
* `financial_valuation_update`
* `trading_history_entry`
* `needs_later_review`

Read the relevant guide before writing. Update only the section that changed — do not regenerate a
file to tidy it.

Note that `trading_history_entry` is usually written **a few days after** the day it describes,
since `follow_through` cannot be known on the day itself. Recording today's notable move is not
urgent.

## Phase 6: Logging

Follow `logs readme` for schemas.

Append to `daily market notes` when there is a meaningful observation worth preserving. Set
`open_question` when the note leaves a specific thread a later run could resolve.

**Do not write to `prediction log` in this program.** Predictions belong to `morning_analysis.md` —
this run records what happened, not what will happen.

## Phase 7: Hand off to analysis

This program does not produce the final report. Carry forward into `morning_analysis.md`:

1. Morning macro/geopolitical context — what changed and why it matters.
2. A-share regime read.
3. Sector/theme findings, with the classifications from Phase 2.
4. Tracked stocks affected, and what was found for each.
5. File updates made, and any flagged as needed but not done.
6. Open questions carried in from Phase 0, and whether today resolved them.
7. Source gaps, blocked sources, and anything that could not be verified.

Keep facts separate from interpretation throughout. The analysis run needs to know what is
established versus what is inferred — if that line is blurred here, the stance built on it will
inherit the error.

If nothing material changed, say so plainly. A quiet morning is a finding, not a failure.

<!-- READ-CHECK: RC-MORNING-N4T7 -->
