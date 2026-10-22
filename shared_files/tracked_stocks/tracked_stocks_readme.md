# tracked_stocks.json rules

Purpose: store tracked stocks in valid JSON.

## Structure
- Top level: sector
- Second level: industry
- Value: array of stock objects

## Stock object
Required:
- `name`: listed company name
- `code`: exchange code, e.g. `688776.SH`
- `notes`: array of short strings

- `tracking`: how closely the stock is followed —
  - `active` — actively traded. Full research and analysis every trading day, including a direct
    announcement check.
  - `passive` — held but not actively traded. Checked only when the day's news touches it or the
    post-close run flags unusual price or volume; reported only when something is found.
  - `off` — no longer followed. Never selected; its profile files are kept.

Optional:
- `tags`: array of short strings
- `upcoming_events`: array of `{ "date": "YYYY-MM-DD", "event": "short description" }` — dated
  future events likely to move the stock: earnings release, 解禁 (lock-up expiry), shareholder
  meeting. Add one when it is found. **Remove an event once its date has passed.**

## Rules
- Keep valid JSON only. No comments.
- Reuse existing sector/industry names when possible.
- Keep notes short, factual, and useful.
- Append new notes; remove outdated notes when clearly obsolete.
- Do not duplicate the same stock code.
- If unsure where a stock belongs, keep the current structure unchanged and add a note instead.

## Example
```json
{
  "Tech": {
    "Semiconductors/半导体": [
      {
        "name": "国光电气",
        "code": "688776.SH",
        "notes": [
          "Track defense electronics demand",
          "Watch next earnings release"
        ],
        "tracking": "active",
        "tags": ["defense", "semiconductor"],
        "upcoming_events": [
          { "date": "2026-10-28", "event": "Q3 report (预约披露)" }
        ]
      }
    ]
  }
}
<!-- READ-CHECK: RC-TRACKED-Z3D7 -->
