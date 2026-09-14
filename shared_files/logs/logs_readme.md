# OpenClaw JSONL Log Format Guide

This file documents the format and rules for all append-only logs in `shared/`.

---

## General Rules

1. Each `.jsonl` file is **one valid JSON object per line**.
2. Never overwrite existing entries. Only append.
3. Use **ISO 8601 timestamps** in UTC: `YYYY-MM-DDTHH:MM:SSZ`.
4. Each entry should include a unique `id` (string) for reference.
5. Optional fields may be `null` if unknown.
6. Linking between logs (predictions → outcomes) uses the field `linked_prediction_id`, carrying
   the `id` of the prediction being evaluated. Use `null` when an entry does not evaluate a
   specific prediction.

---

## prediction_log.jsonl

Each line represents a stock prediction or research note.

**Purpose**

After a stock market prediction is made, it is recorded here to be reviewed later, so the focus is on context, reasoning, and result.

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique identifier for this prediction |
| timestamp | string | ISO 8601 UTC time |
| target_type | string | What is being predicted: `stock`, `sector`, `theme`, or `index`. There is deliberately no `market` value — express a broad call against a specific index so it stays scoreable |
| target_code | string \| null | Stock code (e.g. `"600519"`) or index code. `null` for a sector/theme with no code |
| target_name | string | Human-readable target, e.g. `"贵州茅台"`, `"semiconductors"`, `"上证指数"` |
| horizon | string | Timeframe, e.g. `"open"`, `"morning_session"`, `"full_day"`, `"weekly"` |
| context | string | News and financial context behind the prediction |
| thesis | string | Reasoning — why this outcome follows from the context |
| evidence | array | Specific observations supporting the thesis, each with its source |
| predicted_move | string | Up/down/neutral, outperform/underperform, or a numeric change |
| confidence | number | 0.0–1.0 |
| invalidation | string | Condition that would prove the prediction wrong |
| what_to_check_later | string | What to look at when evaluating this — makes review mechanical |
| self_directed | boolean | `true` if this came from the agent's own initiative rather than a prescribed program step |
| tags | array | Optional list of tags, e.g., `["macro", "earnings"]` |

`target_type` + `target_code` + `target_name` replace the older `stock_code`/`stock_name` pair, so
that a sector or index prediction no longer has to be written as `"stock_code": "SECTOR"`.

**Example:**

```json
{
  "id": "pred-20250617-001",
  "timestamp": "2025-06-17T09:30:00Z",
  "target_type": "sector",
  "target_code": null,
  "target_name": "semiconductors",
  "horizon": "morning_session",
  "context": "Overnight US semi rally on AI capex beat",
  "thesis": "China semi names follow US momentum; opens stronger than broad market",
  "evidence": [
    "SOX +3.1% overnight (Bloomberg, 2025-06-17)",
    "SMIC ADR +4.2% (Reuters, 2025-06-17)"
  ],
  "predicted_move": "outperform",
  "confidence": 0.62,
  "invalidation": "fails to outperform by midday or negative policy/news catalyst appears",
  "what_to_check_later": "半导体 sector index vs 上证指数 at 11:30 close; whether turnover expanded",
  "self_directed": false,
  "tags": ["macro", "sector"]
}
```

---

## market_outcomes.jsonl

Each line represents the actual market outcome corresponding to a prediction. Use `linked_prediction_id` to tie an outcome to a specific prediction when applicable. Use `null` for general market notes that do not evaluate a single prediction.

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique identifier for this outcome |
| timestamp | string | ISO 8601 UTC time |
| target_type | string | Matches the prediction: `stock`, `sector`, `theme`, or `index` |
| target_code | string \| null | Stock or index code; `null` for a sector/theme with no code |
| target_name | string | Human-readable target |
| horizon | string | Timeframe, should match the prediction's horizon |
| actual_move | string | Up/down/neutral or numeric change |
| linked_prediction_id | string \| null | `id` of the prediction this outcome corresponds to, or null |
| notes | string | Optional additional commentary — what happened, and whether the thesis held |

---

## daily_market_notes.jsonl

Each line represents a daily macro, news, or sector observation. Used for the daily pre-market scan and end-of-day summary.

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique identifier for this note |
| timestamp | string | ISO 8601 UTC time |
| type | string | One of: "macro", "sector", "stock", "policy", "sentiment", "summary" |
| subject | string | Topic or ticker, e.g., "US futures", "semiconductors", "600519" |
| content | string | Concise observation or summary |
| sources | array | Optional list of source names/URLs |
| open_question | string \| null | What this note leaves unresolved — e.g. "confirm whether the order announcement is officially disclosed". `null` when the note is self-contained |
| linked_prediction_id | string \| null | Related prediction, if any |

**On `open_question`:** this is what makes the log a working queue rather than a write-only
archive. A thread raised on Monday cannot resurface on Tuesday unless it is marked. Phase 0 of the
morning run scans recent notes for open questions and folds them into what to investigate.

Use it only for a specific question a later run could actually answer. Most notes need none, and
marking everything defeats the purpose.

There is deliberately no `resolved` flag. Closing threads out requires bookkeeping the agent must
remember, and forgotten `open` markers would pile up silently. Open questions instead **age out** —
Phase 0 reads only recent notes, so a thread that stops mattering stops being picked up.

**If something needs to outlive that window, promote it out of the log**: into a logged prediction
with `what_to_check_later`, a watch point in `sectors.md`, or the `news_filter` of a stock profile.
The log is short-term memory, not a permanent to-do list.

Not to be confused with `follow_through` in a stock's `trading_history.jsonl`, which records what
the *price* did after a notable day.

**Example:**

```json
{
  "id": "note-20250617-001",
  "timestamp": "2025-06-17T09:00:00Z",
  "type": "macro",
  "subject": "US futures",
  "content": "S&P 500 futures +0.4% pre-market on tech earnings beat",
  "sources": ["Bloomberg"],
  "linked_prediction_id": null
}
```

---

## weekly_reflections.jsonl

Each line summarizes lessons learned or insights from a weekly review.

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique identifier for reflection |
| timestamp | string | ISO 8601 UTC time |
| summary | string | Summary of what worked, what failed, or notable insights |
| recommendations | string | Suggested improvements for future research |
| linked_predictions | array | List of prediction IDs referenced |
| linked_outcomes | array | List of outcome IDs referenced |
<!-- READ-CHECK: RC-LOGS-H9V4 -->
