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

Optional:
- `status`: `active` or `archived`
- `tags`: array of short strings

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
        "status": "active",
        "tags": ["defense", "semiconductor"]
      }
    ]
  }
}
<!-- READ-CHECK: RC-TRACKED-Z3D7 -->
