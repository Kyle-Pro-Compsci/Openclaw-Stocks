# Morning Research Run

## Purpose
The daily morning run collects important developments since the last run, detects what changed, updates cached research only when needed, and produces concise, falsifiable market observations or predictions.

The process starts wide and narrows, going from:

Global / macro -> Chinese market -> Sector and theme movement -> Tracked Stocks

This program should avoid generic market commentary. It should answer:

1. What changed since the last relevant scan?
2. Which geopolitical, macro, policy, sector, or company developments matter today?
3. Which tracked sectors/stocks are affected?
4. Does the new information change any stock profile, financial view, valuation assumption, market behavior view, or monitoring rule?
5. Are there any specific, falsifiable predictions worth logging?
6. What should be watched during the trading day?


## Phase 0: Setup and recent-state check

First read these files:
`tracked stocks readme` and then `tracked stocks`: To see which stocks to look into.
`weekly reflections`: TODO: Should I move this later? Do I only need this information later when analysis is done, or would it be useful to put it into context now? Could I possibly have notes on things to pay more attention to that would be useful earlier rather than later?
Check recent logs and cached context enough to answer:

* What was already known yesterday or in the last run?
* Are there open predictions that need follow-up?
* Which user research priorities are currently active?
* Which sectors/themes are already marked as hot, warming, cooling, or uncertain?
* Which tracked stocks are active or high priority?

Do not reread large or heavy files unless needed. Only read a file if it's relevant to what you're currently doing.

## Phase 1: Wide macro / geopolitical / economic scan

First read these files:
`user research priorities`: Check what topics the user wants researched as well. TODO: Would this also be better suited to being read in Phase 1? Is there anything actionable in phase 1 or is it just ingesting context?

Start with broad context before looking at individual stocks.

Check for current developments in:

* Major geopolitical events
* U.S. market movement and overnight global risk sentiment
* U.S. rates, dollar, commodities, oil, gold, and major index moves
* China macro, policy, liquidity, regulators, official economic data
* Hong Kong market and China ADR spillover when relevant
* User-priority themes from `user_research_priorities.md`
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

## Phase 2: A-share market regime and sector/theme scan
**TODO:?Read sectors file and edit, use flowchart/step-by-step for whether the sector is present in file, what to do, etc** 
Use the broad context to assess today’s likely A-share setup.

Check:

* Overall market regime: risk-on, risk-off, policy-driven, liquidity-driven, theme-driven, consolidation/震荡, rebound, breakdown risk, etc.
* Major index relevance: 上证指数, 深证成指, 创业板, 科创50, 北证 if relevant.
* Sector and theme heat.
* Whether current hot sectors overlap with tracked industries/stocks.
* Whether current events confirm or contradict existing sector narratives.
* Whether any sector/theme deserves follow-up chained searches.

For each important sector/theme, classify:

* `status`: heating / hot / cooling / exhausted / unclear
* `driver`: policy / macro / earnings / overseas spillover / rumor / technical / liquidity / event
* `quality`: broad-based / narrow leader-driven / speculative / unclear
* `tracked_stock_overlap`: which tracked stocks are relevant
* `follow_up_needed`: yes/no and why

## Phase 3: Tracked stock selection

Read `tracked_stocks.json`.

For the morning run, prioritize:

1. High-priority active stocks.
2. Stocks in sectors/themes affected by the macro/sector scan.
3. Stocks with recent major news, announcements, abnormal price/volume behavior, or stale profiles.
4. Stocks explicitly mentioned by the user’s active research priorities.
5. Stocks with open predictions requiring follow-up.

Do not analyze every stock in full if nothing changed. For unaffected stocks, it is acceptable to note “no material new information found” if checked.

## Phase 4: Stock profile/cache check

For each selected stock:

1. Check whether a stock profile folder exists.
2. If no profile exists, create or flag need for baseline creation using the stock profile templates.
3. If a profile exists, read `stock_profile.json` first.
4. Use the stock profile to decide whether heavier files are needed.

Read `financial snapshot` only when: (TODO: Referring to something in the stock profile, not in paths.json, look into this)

* Financial quality matters.
* Earnings, revenue, margins, cash flow, balance sheet, or concept revenue exposure matters.
* New information claims the business is improving or deteriorating.
* The market narrative depends on whether the company is a real beneficiary or only a proxy/concept stock.

Read `valuation_baseline.json` only when:

* Valuation, price level, upside/downside, or rerating matters.
* Price has moved sharply.
* New information changes growth, margin, multiple, or market-implied assumptions.
* User asks whether a move is justified.

Read market behavior / trading behavior profile if available when:

* There is a sharp move, gap, limit-up/limit-down, volume/turnover jump, breakout/breakdown, sector divergence, or intraday trading relevance.
* Recent price behavior matters more than financial details.

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


### Phase 5: Change detection

For each relevant stock or sector, ask:

1. Does today’s information change the current market narrative?
2. Does it affect watched factors?
3. Does it affect financial quality or business support for the story?
4. Does it affect valuation assumptions or what the market is pricing in?
5. Does it affect market behavior or trading personality?
6. Does it create, remove, or change monitoring rules?
7. Does it confirm prior assumptions without requiring an update?
8. Does it contradict prior assumptions and require correction?

Only update cached files when a specific section has changed or is stale. Do not rewrite full profiles just to make them look cleaner.

### Phase 6: Logging rules

Append to `daily_market_notes.jsonl` when there is a meaningful market, macro, sector, or stock note worth preserving.

Append to `prediction_log.jsonl` only when the prediction is specific and falsifiable.

A prediction must include:

* Target: index, sector, theme, or stock
* Horizon: intraday / morning session / full trading day / weekly / other
* Expected move or scenario
* Thesis
* Evidence
* Confidence
* Invalidation condition
* What to check later

Do not force a prediction when evidence is weak. It is acceptable to say there is no high-quality prediction.

Do not rewrite old prediction entries. If later evaluation is needed, append to `market_outcomes.jsonl`.

### Phase 7: Output summary

Return a concise supervisor-ready report with these sections:

1. Morning macro/geopolitical context
2. A-share regime / likely market setup
3. Sector/theme focus
4. Tracked stocks affected
5. Profile/cache updates made or needed
6. Predictions logged
7. Watchlist for today
8. Source gaps / uncertainty / blocked sources

The report should clearly separate:

* Facts
* Interpretation
* Prediction
* Uncertainty

Avoid generic filler. If nothing material changed, say so directly.
