# OpenClaw JSONL Log Format Guide

This file documents the format and rules for all append-only logs in `shared/`.

---

## General Rules

1. Each `.jsonl` file is **one valid JSON object per line**.
2. Never overwrite existing entries. Only append.
3. Use **ISO 8601 timestamps** in UTC: `YYYY-MM-DDTHH:MM:SSZ`.
4. Each entry should include a unique `id` (string) for reference.
5. Optional fields may be `null` if unknown.
6. Linking between logs (predictions → outcomes) should use `prediction_id`.

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
| stock_code | string | Stock code, e.g., "600519" |
| stock_name | string | Human-readable stock name |
| horizon | string | Timeframe of prediction, e.g., "5min", "daily", "weekly" |
| context | string | News and financial context behind the prediction |
| thesis | string | Analyst reasoning or research notes |
| predicted_move | string | Up/down/neutral or numeric change |
| confidence | number | 0.0–1.0 |
| invalidation | string | Condition that would prove the prediction wrong |
| tags | array | Optional list of tags, e.g., ["macro", "earnings"] |

**Example:**

```json
{
  "id": "pred-20250617-001",
  "timestamp": "2025-06-17T09:30:00Z",
  "stock_code": "SECTOR",
  "stock_name": "semiconductors",
  "horizon": "next_trading_day_morning",
  "context": "Overnight US semi rally on AI capex beat",
  "thesis": "China semi names follow US momentum; opens stronger than broad market",
  "predicted_move": "outperform",
  "confidence": 0.62,
  "invalidation": "fails to outperform by midday or negative policy/news catalyst appears",
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
| stock_code | string | Stock code |
| stock_name | string | Stock name |
| horizon | string | Timeframe, should match prediction horizon |
| actual_move | string | Up/down/neutral or numeric change |
| linked_prediction_id | string \| null | ID of prediction this outcome corresponds to, or null |
| notes | string | Optional additional commentary |

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
| linked_prediction_id | string \| null | Related prediction, if any |

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