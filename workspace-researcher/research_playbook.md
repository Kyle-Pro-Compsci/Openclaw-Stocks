# Research Playbook
TODO: This should contain general research methodology
TODO: Sources and research should all be usable by Chinese Kimi (is there a difference with an international version? )


Purpose: Produce repeatable stock research with current information, explicit reasoning, and weekly self-review.

## Global Rules

- When beginning a research task send the user the message "Following research_playbook rules" to signal that this file has been read.
- Do not use out-of-date sources. Check the date of every source used.
- Do not fail silently. If a source cannot be accessed or you are blocked, report this explicitly.
- Track and report sources used for individual pieces of information.
- Do not hallucinate. Do not fabricate results that "sound right."
- Separate facts, interpretation, prediction, and uncertainty clearly.
- Label rumors as rumors; do not convert rumor into fact.
- Only log predictions that are specific and falsifiable.
- Do not repeat yesterday’s analysis unless something is still relevant and the reason is stated.

## Required Files to Consult

Before beginning any research pass, read:

- `~/.openclaw/shared_files/paths.json` — shared file index and locations
- `~/.openclaw/shared_files/tracked_stocks/tracked_stocks.json` — current watchlist and sector map
- `~/.openclaw/shared_files/tracked_stocks/tracked_stocks_readme.md` — how to read/write the watchlist
- `~/.openclaw/shared_files/user_research_priorities.md` — user-defined priority topics
- `~/.openclaw/shared_files/learned_research_lessons.md` — durable lessons from past research
- `~/.openclaw/shared_files/source_guide.md` — source hierarchy and credibility rules

## Methodology: Top-Down Research

1. Start broad: global macro and market regime (e.g., 上证指数, S&P 500, VIX).
2. Narrow to sectors/themes relevant to `tracked_stocks.json` and `user_research_priorities.md`.
3. Drill to industries and individual stocks.
4. Use `tracked_stocks.json` to decide which sectors, industries, and stocks deserve attention.
5. Start daily research from the latest weekly baseline when available.

## Chained Follow-Up Searches

When a market-moving thread appears:

- Dig deeper with follow-up searches.
- Stop chaining when the claim is verified, contradicted, still unverified/rumor, or irrelevant to tracked sectors/stocks.
- Record the chain and conclusion in daily notes.


---

## Research Programs (To move into individual files? Read this one first then path to file)

### 1. Morning Research Run

**Purpose**
The daily morning run that collects all breaking news since the last run, detects what changed, updates cached research only when needed, and produces concise, falsifiable market observations or predictions. It starts wide, looking into global geopolitic news, and narrows into financial news on tracked sectors/stocks.



---

## Daily Workflow Temporary Outlines

### 1. Daily Pre-Market Scan

- Read `user_research_priorities.md` and `learned_research_lessons.md`.
- Read the latest weekly reflection (if any) to establish baseline assumptions.
- Check `tracked_stocks.json` for active sectors/industries.
- Note macro overnight moves (US, Europe, Asia futures, FX, commodities, bonds).
- Record findings in `daily_market_notes.jsonl`.

### 2. News / Macro Research Pass

- Focus on geopolitical, policy, and macro events that could move tracked markets.
- Validate that saved baseline assumptions (e.g., sector hotspots) are still valid.
- Update or invalidate prior assumptions with evidence.
- Record key developments in `daily_market_notes.jsonl`.

### 3. Sector / Theme Research Pass

- Scan tracked sectors for new catalysts, policy shifts, or sentiment changes.
- Identify industry hotspots (e.g., semiconductors) with specific evidence.
- Note sector rotation signals.

### 4. Company / Financial Research Pass

- For individual stocks in `tracked_stocks.json`:
  - Review recent filings, earnings, and announcements.
  - Check brokerage summaries and reputable media coverage.
  - Update cached info only if new news arrives or a set review period has passed.

### 5. Collation and Prediction Pass

- Synthesize findings into concise, actionable summaries.
- Form falsifiable predictions with:
  - **target** (stock, sector, or index)
  - **horizon** (timeframe)
  - **thesis** (reasoning)
  - **confidence** (0.0–1.0)
  - **invalidation_condition** (what would prove this wrong)
- Log predictions to `prediction_log.jsonl`.

### 6. End-of-Day Review

- Compare market outcomes to morning predictions.
- Log outcomes to `market_outcomes.jsonl`.
- Link outcomes to `prediction_id` when evaluating a specific prediction; use `null` for general market notes.
- Append a brief daily summary to `daily_market_notes.jsonl`.

---

## Weekly Research / Reflection

Once per week (or per standing order / cron trigger):

1. Read all `daily_market_notes.jsonl` entries from the week.
2. Read all predictions and outcomes from the week.
3. Summarize what worked, what failed, and notable insights.
4. Write the weekly summary to `weekly_reflections.jsonl`.
5. Distill durable lessons into `learned_research_lessons.md`.
   - Raw weekly reflections belong in `weekly_reflections.jsonl`.
   - Curated, long-term lessons belong in `learned_research_lessons.md`.
   - Do not dump raw reflections into `research_playbook.md` or `AGENTS.md`.

---

## Output Requirements

- Prefer concise, actionable summaries unless depth is requested.
- Include source citations with dates/freshness.
- Clearly label facts vs. interpretation vs. prediction vs. uncertainty.
- Report blocked, inaccessible, or failed sources/tools explicitly.
- Always identify what changed since the last relevant scan.

## Logging Requirements

Use the shared JSONL logs under `~/.openclaw/shared_files/logs/`:

- `prediction_log.jsonl` — specific, falsifiable predictions
- `market_outcomes.jsonl` — actual outcomes; link to `prediction_id` when relevant
- `daily_market_notes.jsonl` — daily macro, news, and sector notes
- `weekly_reflections.jsonl` — weekly review summaries

All logs are append-only. Never overwrite entries.

## Failure Handling

- If a required file is missing, report it and continue with available files.
- If web search fails, state the failure explicitly and note what you could not verify.
- If a source is inaccessible, do not substitute with unverified speculation.
- When uncertain, state the uncertainty rather than inventing a conclusion.

---

## TODO: Future Refinement

- [ ] Refine China-market source selection and trusted outlets.
- [ ] Define exact review cadence for cached stock info.
- [ ] Add sector rotation scoring or heat-map methodology.
- [ ] Expand market regime tracker (consolidation, bull, bear) with specific signals.
- [ ] Document real-analyst workflow parallels if/when known.
