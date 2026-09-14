# Morning Research Run

## Purpose

The daily morning run collects important developments since the last run, detects what changed, updates cached research only when needed, and produces concise, falsifiable market observations or predictions.

The process starts wide and narrows:

global / geopolitical / macro
→ China market regime
→ sector and theme movement
→ tracked stocks
→ profile/cache updates
→ logs and summary

This program should avoid generic market commentary. It should answer:

1. What changed since the last relevant scan?
2. Which geopolitical, macro, policy, sector, or company developments matter today?
3. Which sectors/themes are affected?
4. Which tracked stocks deserve attention today?
5. Does the new information change any sector view, stock profile, financial view, valuation assumption, market behavior view, or monitoring rule?
6. Are there any specific, falsifiable predictions worth logging?
7. What should be watched during the trading day?

## File responsibility rule

This program controls the research flow.

Do not duplicate detailed file-writing rules here. Use the relevant file guide:

* Use `sectors_readme.md` for when and how to update `sectors.md`.
* Use `logs_readme.md` for when and how to append JSONL logs.
* Use stock profile templates/readmes for when and how to update stock profile files.
* Use `source_guide.md` for source reliability.
* Use `paths.json` to locate shared files.

During the run, collect candidate updates. Only write to a file when the relevant file guide says the update belongs there.

## Required files to consult

Before execution, read or locate through `~/.openclaw/shared_files/paths.json`:

* `source guide`
* `user research priorities`
* `learned research lessons`
* `tracked stocks`
* `logs readme`
* sector files, if present:

  * `sectors readme`
  * `sectors`

When relevant, also read:

* recent `daily_market_notes.jsonl`
* recent `prediction_log.jsonl`
* recent `market_outcomes.jsonl`
* existing stock profile files for selected active tracked stocks

## Phase 0: Setup and recent-state check

Use `paths.json` to locate required files.

Check recent logs and cached context enough to answer:

* What was already known yesterday or in the last run?
* Are there open predictions that need follow-up?
* Which user research priorities are currently active?
* Which sectors/themes are already marked as hot, warming, cooling, or uncertain?
* Which tracked stocks are active or high priority?

Do not reread large or heavy files unless needed. Only read a file if it's relevant to what you're currently doing.

## Phase 1: Wide macro / geopolitical / economic scan

Start with broad context before looking at individual stocks.

Check for current developments in:

* major geopolitical events;
* U.S. market movement and overnight global risk sentiment;
* U.S. rates, dollar, commodities, oil, gold, and major index moves;
* China macro, policy, liquidity, regulators, and official economic data;
* Hong Kong market and China ADR spillover when relevant;
* user-priority themes from `user_research_priorities.md`;
* any urgent event likely to affect A-share open, sector rotation, risk appetite, or policy expectations;
* at least one additional topic judged relevant by the Researcher.

Output of this phase should be a concise morning context summary:

* what changed;
* why it matters;
* affected markets;
* affected sectors/themes;
* confidence;
* source notes;
* uncertainties.

Do not turn weak rumors into facts. Label rumors, blocked sources, and source gaps clearly.

If Phase 1 reveals information that may change a sector view, create a candidate sector update. Do not write immediately unless necessary.

## Phase 2: A-share market regime and sector/theme scan

Use the broad context to assess today’s likely A-share setup.

Check:

* broad market regime: risk-on, risk-off, policy-driven, liquidity-driven, theme-driven, consolidation/震荡, rebound, breakdown risk, etc.;
* major index relevance: 上证指数, 深证成指, 创业板, 科创50, 北证 if relevant;
* sector and theme heat;
* whether current hot sectors overlap with user priorities or tracked-stock sectors;
* whether current events support, weaken, or complicate existing sector views;
* whether any sector/theme deserves chained follow-up research.

For each important sector/theme, classify:

* `heat_status`: ignored / warming / hot / cooling / exhausted / reactivating / unclear
* `driver`: policy / macro / earnings / overseas spillover / liquidity / event / speculation / unclear
* `move_quality`: broad / leader-led / follower-led / speculative / weak / mixed / unclear
* `current_view_change`: no change / supported / weakened / changed / unclear
* `attention_change`: more attention / less attention / same attention / unclear
* `follow_up_needed`: yes/no and why

Use `sectors_readme.md` to decide whether the result belongs in `sectors.md` or only in `daily_market_notes.jsonl`.

Do not add stock-specific details to `sectors.md`. Stock-specific sector role belongs in the stock profile.

## Phase 3: Tracked stock selection

Read `tracked_stocks.json`.

Prioritize stocks for deeper review when they match one or more of these:

1. High-priority active stocks.
2. Stocks in sectors/themes affected by Phase 1 or Phase 2.
3. Stocks with recent major news, announcements, abnormal price/volume behavior, or stale profiles.
4. Stocks explicitly connected to active user research priorities.
5. Stocks with open predictions requiring follow-up.

Do not analyze every stock in full if nothing changed.

For unaffected stocks, it is acceptable to skip them or state that no material new information was found.

## Phase 4: Stock profile/cache check

For each selected stock:

1. Check whether a stock profile folder exists.
2. If no profile exists, create or flag the need for baseline creation using the stock profile templates.
3. If a profile exists, read `stock_profile.json` first.
4. Use the stock profile to decide whether heavier files are needed.

Read the financial snapshot only when:

* financial quality matters;
* earnings, revenue, margins, cash flow, balance sheet, or concept revenue exposure matters;
* new information claims the business is improving or deteriorating;
* the market narrative depends on whether the company is a real beneficiary or only a proxy/concept stock.

Read the valuation baseline only when:

* valuation, price level, upside/downside, or rerating matters;
* price has moved sharply;
* new information changes growth, margin, multiple, or market-implied assumptions;
* the research question depends on whether a move is justified.

Read the market behavior profile when:

* there is a sharp move, gap, limit-up/limit-down, turnover jump, breakout/breakdown, sector divergence, or intraday trading relevance;
* recent price behavior matters more than financial details.

## Chained follow-up search rule

This rule can trigger during any phase.

If a search reveals a potentially market-moving thread, run follow-up searches instead of stopping at the first result.

Continue follow-up until one of these is true:

1. The claim is verified by stronger sources.
2. The claim is contradicted.
3. The claim remains unverified and must be labeled as rumor or low-confidence.
4. The thread is not relevant to current sectors, user priorities, or tracked stocks.
5. Additional searches no longer produce materially new information.

For each chained thread, record:

* original trigger;
* strongest confirming source;
* strongest contradicting source, if any;
* current confidence;
* market relevance;
* affected sectors/themes;
* affected tracked stocks, if any;
* whether a file update is needed.

## Phase 5: Change detection and write decisions

For each relevant macro item, sector, theme, or stock, ask:

1. Is this new, or was it already known?
2. Does it change the current view?
3. Does it change heat status, sector driver, move quality, stock narrative, watched factors, valuation assumptions, financial quality, or market behavior?
4. Is the evidence strong enough to preserve in a baseline file?
5. Is it only a daily observation?
6. Is it specific enough to become a falsifiable prediction?

Classify each candidate update:

* `no_write`
* `daily_market_note`
* `sector_update`
* `stock_profile_update`
* `financial_snapshot_update`
* `valuation_update`
* `market_behavior_update`
* `prediction_log_entry`
* `outcome_log_entry`
* `needs_later_review`

Use the relevant file guide before writing.

Do not rewrite full files just to make them cleaner. Update only the specific section that changed or is stale.

## Phase 6: Logging

Use `logs_readme.md` for exact JSONL schemas.

Append to `daily_market_notes.jsonl` when there is a meaningful market, macro, sector, or stock observation worth preserving.

Append to `prediction_log.jsonl` only when the prediction is specific and falsifiable.

A prediction must include:

* target;
* horizon;
* expected move or scenario;
* thesis;
* evidence;
* confidence;
* invalidation condition;
* what to check later.

Do not force a prediction when evidence is weak.

Do not rewrite old prediction entries. If later evaluation is needed, append to `market_outcomes.jsonl`.

## Phase 7: Output summary

Return a concise supervisor-ready report with these sections:

1. Morning macro/geopolitical context
2. A-share regime / likely market setup
3. Sector/theme focus
4. Tracked stocks affected
5. File updates made or needed
6. Predictions logged
7. Watchlist for today
8. Source gaps / uncertainty / blocked sources

The report should clearly separate:

* facts;
* interpretation;
* prediction;
* uncertainty.

Avoid generic filler. If nothing material changed, say so directly.
