# Research Playbook

## Purpose

This playbook defines the general research standards, reasoning habits, and file-use rules for the Researcher agent.

It is not a step-by-step cron program. Specific workflows, such as the daily morning run, should live in individual program files such as:

- `workspace-researcher/programs/morning_research_run.md`

Use this playbook for every macro, sector, theme, and stock research task. Use the specific program file for the execution order of a scheduled run.

The goal is to produce repeatable stock research with current information, explicit reasoning, controlled file updates, and reviewable predictions.

## Relationship to other files

Use these files according to their role. Do not duplicate their detailed rules inside this playbook.

- `~/.openclaw/shared_files/paths.json` — source of truth for shared file names and locations.
- `~/.openclaw/shared_files/source_guide.md` — source hierarchy, credibility rules, and source-quality handling.
- `~/.openclaw/shared_files/user_research_priorities.md` — user-defined topics and standing research priorities.
- `~/.openclaw/shared_files/learned_research_lessons.md` — curated durable lessons from past research.
- `~/.openclaw/shared_files/tracked_stocks/tracked_stocks_readme.md` — rules for reading or editing the tracked stock list.
- `~/.openclaw/shared_files/tracked_stocks/tracked_stocks.json` — current watchlist, sector map, stock status, and monitoring tiers.
- `~/.openclaw/shared_files/logs/logs_readme.md` — exact JSONL log schemas and append rules.
- `~/.openclaw/shared_files/logs/daily_market_notes.jsonl` — daily macro, market, sector, and stock observations.
- `~/.openclaw/shared_files/logs/prediction_log.jsonl` — specific, falsifiable predictions.
- `~/.openclaw/shared_files/logs/market_outcomes.jsonl` — later outcomes and prediction evaluations.
- `~/.openclaw/shared_files/logs/weekly_reflections.jsonl` — weekly review records.
- `~/.openclaw/shared_files/sectors/sectors_readme.md` — rules for maintaining `sectors.md`.
- `~/.openclaw/shared_files/sectors/sectors.md` — compact sector state and sector-view file.
- `~/.openclaw/shared_files/stock_profiles/stock_profiles_readme.md` — rules for stock profile folders and profile/cache files.

When a file is referenced by a friendly key such as `tracked stocks`, resolve it through `paths.json` if possible.

## File access principles

Read files just-in-time.

Do not load every possible file at the start of a task. Read early only when the file changes what the Researcher should pay attention to. Read detailed files near the decision that needs them.

General timing:

- Read `paths.json` at the start of any task that uses shared files.
- Read `user_research_priorities.md` near the start of broad market or daily research.
- Read `learned_research_lessons.md` near the start of research, but use it as curated guidance, not as a raw history log.
- Read `tracked_stocks_readme.md` before editing `tracked_stocks.json`; read `tracked_stocks.json` when selecting sectors/stocks.
- Read `source_guide.md` before or during source-heavy research and whenever source quality is being judged.
- Read `sectors_readme.md` before editing `sectors.md`.
- Read `logs_readme.md` before appending to any JSONL log.
- Read stock profile files only after a stock has been selected for deeper review.
- Read financial, valuation, or market behavior files only when the task specifically requires those details.

If an exact rule, schema, or field matters, reread the relevant source file before acting instead of relying on memory.

## Global research rules

- Do not hallucinate. Do not fabricate information, sources, figures, or market reactions.
- Do not fail silently. If a source, tool, file, or website cannot be accessed, report the limitation.
- Do not use out-of-date sources without labeling them as historical.
- Check the date or reporting period of every source used.
- Track which source supports which important claim.
- Separate facts, interpretation, prediction, and uncertainty.
- Label rumors and unverified claims clearly.
- Do not convert rumor, market chatter, or social-media sentiment into fact.
- Prefer concise, actionable conclusions over long generic summaries.
- Do not repeat prior analysis unless it remains relevant and you state why.
- If nothing material changed, say so directly.

## Source discipline

Use `source_guide.md` as the authority for source reliability.

General standards:

- Prefer official and primary sources for company-specific facts: exchange disclosures, company announcements, filings, regulators, official macro releases, and official policy documents.
- Use financial media and market-data portals for discovery, context, market sentiment, sector movement, and timely summaries.
- Treat social media, forums, screenshots, and unsourced market chatter as low-confidence discovery leads unless verified by stronger sources.
- For China A-share research, prioritize sources that Kimi/domestic search can realistically access and verify. Chinese-language official and financial sources are often more practical than Western sources.
- For global macro and overseas spillover, use English or international sources when available, but do not assume every Western source is accessible.
- If Kimi search returns a synthesized answer, identify the underlying sources if possible. Treat the synthesized answer as a discovery layer unless source names, dates, and links are clear.
- When source access is blocked or incomplete, state what could not be verified.

## Core methodology: top-down, then stock-specific

Use a top-down funnel unless the user asks a narrowly stock-specific question.

1. Start with external and macro context.
   - Global risk sentiment, geopolitical events, U.S. market movement, rates, FX, commodities, oil, gold, and major policy developments.
2. Translate broad context into China/A-share relevance.
   - Ask how the information affects risk appetite, liquidity, policy expectations, sector rotation, and index behavior.
3. Identify affected sectors and themes.
   - Focus on sectors relevant to user priorities, tracked stocks, current market heat, or major new catalysts.
4. Narrow to industries and stocks.
   - Use `tracked_stocks.json` to identify active/watch/passive stocks and relevant sector mappings.
5. Check stock-specific relevance.
   - Use stock profiles to decide whether new information is actually relevant, already known, exaggerated, or worth deeper analysis.
6. Update only what changed.
   - Do not regenerate baseline research unless the profile is missing, stale, contradicted, or materially incomplete.

## Change detection and novelty

Every research task should ask:

- What is new since the last relevant scan?
- Was this already known or already reflected in the current view?
- Does it change the market regime, sector view, stock narrative, watch factors, valuation assumptions, financial quality, or market behavior?
- Does it confirm an existing view without requiring an update?
- Does it weaken or contradict an existing view?
- Is the evidence strong enough to preserve in a baseline file?
- Is this only a daily observation?
- Is this specific enough to become a falsifiable prediction?

Do not write to files just to show work. Write only when the information belongs in that file.

## Sector research rules

Use sector research to understand sector heat, rotation, movement quality, and how sector-level movement may affect stocks inside the sector.

When researching a sector/theme, focus on:

- current heat status: ignored, warming, hot, cooling, exhausted, reactivating, or unclear;
- main driver: policy, macro, earnings, overseas spillover, liquidity, event, speculation, commodity price, or unclear;
- move quality: broad, leader-led, follower-led, speculative, weak, mixed, or unclear;
- current sector view;
- observed evidence supporting the current view;
- observed evidence weakening or complicating the current view;
- watch points for strengthening, weakening, rotation, or exhaustion;
- what headlines/noise should be ignored unless confirmed.

Do not put stock-specific role details in `sectors.md`. Stock-specific sector role belongs in the stock profile.

Use `sectors_readme.md` before editing `sectors.md`.

## Stock research rules

Use stock research to answer:

- What does the company actually do?
- Why does the stock move?
- Which sector/theme bucket does the market trade it under?
- Is it a real beneficiary, proxy stock, concept stock, leader, follower, bottleneck supplier, or sector anchor?
- What kind of news is relevant?
- What should be ignored unless confirmed?
- Does new information change the stock narrative, watched factors, financial quality, valuation assumptions, or market behavior?

For stock-specific research, read `stock_profile.json` first when it exists.

Use the profile to decide whether heavier files are needed:

- Read the financial snapshot only when financial quality, earnings, revenue, margin, cash flow, balance sheet, or real concept exposure matters.
- Read the valuation baseline only when valuation, price level, rerating, upside/downside, or market-implied assumptions matter.
- Read the market behavior profile only when recent price/volume behavior, limit-up/limit-down behavior, key levels, sector divergence, or intraday trading behavior matters.

Do not analyze every tracked stock deeply every day. Prioritize based on monitoring tier, sector relevance, direct news, abnormal behavior, stale profiles, user priorities, and open predictions needing follow-up.

## Financial and valuation research rules

Financial and valuation research should answer whether the market story is supported by business evidence.

When financial quality matters, look for:

- revenue and profit trend;
- deducted net profit / 扣非归母净利润;
- gross margin and net margin;
- operating cash flow and cash conversion;
- receivables, inventory, goodwill, debt, and asset-liability ratio;
- government subsidies or non-recurring gains that distort profit;
- customer concentration, order quality, and concept revenue exposure;
- ST risk, shareholder reduction, pledged shares, lock-up pressure, and exchange inquiries where relevant.

When valuation matters, prefer practical ranges and assumptions over fake precision.

Useful valuation questions:

- Is the stock cheap, fair, expensive, or speculative relative to its business and sector?
- What is the market currently pricing in?
- What growth, margin, multiple, or narrative premium seems required?
- What would revalue or devalue the stock?
- What would invalidate the valuation view?

Avoid full DCF modeling unless the business is stable enough and the user specifically needs it.

## Market behavior and trading relevance

For short-term A-share research, recent trading behavior can be as relevant as financial detail.

When market behavior matters, look for:

- recent highs/lows and key levels;
- limit-up / limit-down behavior;
- volume or turnover jumps;
- breakout, breakdown, failed breakout, or intraday fade;
- high-open-low-close / low-open-high-close behavior;
- whether the stock leads, follows, lags, or diverges from its sector;
- whether the move is leader-led, follower-led, speculative, or liquidity-driven;
- whether the stock tends to spike and fade, trend steadily, or move only with themes.

Do not store raw minute-level data unless a specific program or user request requires it. Store interpreted trading behavior and notable events.

## Chained follow-up searches

Use follow-up searches only for claims that may materially affect today’s market, an important sector view, a tracked stock, or a prediction.

Do not chase every interesting thread.

Default budget:

- up to 2 follow-up searches per thread;
- up to 3 chained threads per run;
- exceed this only when the issue is clearly market-moving and state why.

Stop when:

- a stronger source verifies the claim;
- a stronger source contradicts the claim;
- the claim remains unverified and should be labeled rumor/low-confidence;
- the thread is no longer relevant to current sectors, user priorities, or tracked stocks;
- additional searching is not producing materially new information.

For important followed threads, keep a short working note:

- claim;
- best source found;
- confidence;
- affected sectors/stocks;
- write decision.

## Prediction rules

Only make predictions when they are specific enough to evaluate later.

A prediction should include:

- target: index, sector, theme, or stock;
- horizon: intraday, morning session, full trading day, weekly, or other;
- expected move or scenario;
- thesis;
- evidence;
- confidence;
- invalidation condition;
- what to check later.

Do not force predictions from weak evidence. It is acceptable to say there is no high-quality prediction.

Predictions belong in `prediction_log.jsonl`, following `logs_readme.md`.

## Logging and file update rules

Use the relevant file guide before writing.

- Use `logs_readme.md` before appending to JSONL logs.
- Use `sectors_readme.md` before editing `sectors.md`.
- Use `tracked_stocks_readme.md` before editing `tracked_stocks.json`.
- Use `stock_profiles_readme.md` before creating or editing stock profile/cache files.

Keep raw evidence logs append-only unless their specific readme says otherwise.

Use this split:

- `daily_market_notes.jsonl` — meaningful daily observations that may help future comparison.
- `prediction_log.jsonl` — falsifiable predictions only.
- `market_outcomes.jsonl` — later outcomes and prediction evaluations.
- `weekly_reflections.jsonl` — weekly review records.
- `learned_research_lessons.md` — curated durable lessons only.
- `sectors.md` — compact sector baseline and current sector view.
- stock profiles — stock-specific relevance, financial/valuation digest, and market behavior cache.

Do not dump raw weekly reflections into `research_playbook.md` or `AGENTS.md`.

## Output requirements

Research output should be concise but not shallow.

Include:

- what changed;
- why it matters;
- affected markets, sectors, themes, or stocks;
- source quality and freshness;
- facts versus interpretation;
- prediction versus uncertainty;
- what to watch next;
- file updates made or recommended, when relevant.

Avoid:

- generic market commentary;
- unsupported conclusions;
- repeating stale narratives;
- overconfident claims based on weak sources;
- long summaries that do not affect decisions.

## Failure handling

If a required file is missing, report it and continue with available files.

If web search fails, say what failed and what could not be verified.

If a source is inaccessible, do not substitute speculation.

If sources conflict, report the conflict and prefer stronger sources according to `source_guide.md`.

If uncertain, state uncertainty instead of inventing a conclusion.

## Relationship to research programs

This playbook provides general rules and methodology.

Specific programs should define execution order. Examples:

- `workspace-researcher/programs/morning_research_run.md` — daily morning research run.

A program may override the order of operations for its specific purpose, but it should not override the source, evidence, logging, or hallucination rules in this playbook unless the user explicitly instructs otherwise.

## Future refinement

- Refine China-market source selection and trusted outlets.
- Define exact review cadence for cached stock information.
- Add sector rotation or heat-map methodology if useful after testing real runs.
- Expand market regime tracking with concrete signals if the current labels are too vague.
- Improve stock profile templates after observing which fields the Researcher actually uses.
