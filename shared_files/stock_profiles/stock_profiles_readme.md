# Stock Profiles Guide

Cached per-stock research, so a company is not re-researched from scratch every day. Daily work asks
"what changed?" and updates only the section that changed.

## Folder naming

One folder per stock, named by the **bare 6-digit code with no suffix**: `688776`, `600519`.

A-share codes are partitioned across exchanges (600/601/603/605 SSE, 688 STAR, 000/001/002/003
SZSE, 300/301 ChiNext, 43/83/87/920 BSE), so a bare code cannot collide.

The reason for the bare code rather than `688776_国光电气` is **stability**: 股票简称 changes when
a company is ST'd or rebrands, which would either strand the folder or force a rename that breaks
every path pointing at it. The code never changes.

Strip the `.SH` / `.SZ` / `.BJ` suffix when building the path from `tracked_stocks.json`. Exchange
is recorded inside `stock_profile.json`.

## The three files

Created from the templates in `_templates/`, resolved through `paths.json`.

| File | Holds | Read it when |
|---|---|---|
| `stock_profile.json` | Current judgment: what it is, why it moves, how it trades, the story now, what matters and what to ignore | **Always first**, for any stock-specific task |
| `financial_valuation_snapshot.json` | The numbers: financial quality, and what the current price implies | Financial quality, earnings, margins, cash flow, balance sheet, real concept exposure, valuation, or rerating matters |
| `trading_history.jsonl` | The raw record of notable trading days | Price levels, prior reactions, or whether a move is likely to hold matter |

The split is **current judgment vs. numbers vs. accumulating record**:

- `stock_profile.json` holds everything needed to judge whether a stock matters today and what it is
  likely to do — including how it trades. Trading personality ("fades after limit-up") is identity,
  not history, so it lives here. Key levels live here too; they change often, but they are two or
  three fields.
- `financial_valuation_snapshot.json` keeps financials and valuation **together**: valuation
  assumptions only mean anything next to the financials that justify them, and splitting them leaves
  one half stale while the other is fresh.
- `trading_history.jsonl` is the only thing that genuinely accumulates, so it is the only thing
  separated out.

The profile holds **verdicts**; the history holds the **record** those verdicts are derived from.
Do not copy raw days into the profile, and do not write forward-looking views into the history.

`stock_profile.json` should stay **short and qualitative**. Prefer a clear sentence over a
half-filled structure. `unknown` is a legitimate value; an invented one is corruption.

Stock-specific sector role belongs **here**, not in `sectors.md`.

### What `trading_history.jsonl` is for

A price level on its own is trivia. A level with the reaction attached is tradeable. The point of
the history is to keep the two together.

What you get back from it:

1. **Peaks, lows, and pressure points** — where the stock has actually turned. These are the levels
   that matter today, and they exist nowhere else.
2. **What happened at each** — rejected cleanly, broke and held, or broke and failed back. Three
   very different futures from the same price.
3. **Whether moves stick** — does a big up-day hold or give it back? In A-shares that is the
   difference between chasing a move and fading it.
4. **What kind of news actually moves it** — qualitatively. "Policy headlines barely register, order
   announcements move it hard" is established by a handful of entries and is directly useful.
5. **Whether it still behaves as the profile claims** — the profile's personality verdict is derived
   from these entries. When the stock changes character it shows here first, and that is the signal
   the profile has gone stale.

**Do not use it to infer what is normal.** Every entry is by definition an abnormal day, so it is a
biased sample. Judging typical turnover or average daily range from this file will be wrong — take
those from actual market data.

### History vs. the shared logs

Both record things that happened, but they are reached by different questions: **history is indexed
by stock, the logs are indexed by time.** "What has this stock done at ¥18 before?" is history.
"What did we think last Tuesday, and were we right?" is the logs.

The content rule is **settled vs. pending**:

- **Settled** — about a day that has already happened and will not change → history. This includes
  interpretation: what happened at the level, the likely cause, whether it followed through. Reading
  the tape is part of the record.
- **Pending** — only gradeable by what happens next → the logs. Views, theses, predictions.

So a judgment *of the day* belongs in history; a judgment *about the future* does not. "Broke the
March high on heavy volume and held" is history. "This is set up to run" is a view, and belongs in
`daily market notes` or `prediction log` where it can be scored.

### `trading_history.jsonl` entry format

One JSON object per line, append-only, oldest first. **Weight recent entries well above older
ones** — behavior from many months ago is usually irrelevant to how the stock trades today.

| Field | Description |
|---|---|
| `date` | `YYYY-MM-DD` |
| `event_types` | One or more of: `limit_up`, `limit_down`, `gap`, `breakout`, `breakdown`, `reversal`, `turnover_spike`, `divergence`. If none fits, use `other` and describe the day in `note` |
| `price_move` | The day's move, e.g. `"+9.98%"` |
| `level_context` | The price level the stock was testing that day, and what it did there — e.g. "reached the March high of ¥18.40, closed below it". `null` if no known level was in play |
| `volume_note` | How turnover compared with its own recent norm |
| `sector_context` | What the sector did that day — did it lead, follow, or diverge? |
| `likely_cause` | The catalyst, if identifiable. `unknown` is acceptable |
| `follow_through` | What the price did over the next day or two — held, faded, continued |
| `note` | Anything else worth remembering |

**Does this day qualify?** Record a day if it tells you something about how this stock trades: how
it reacts at a price level, what kind of news moves it, or whether its big moves hold.

As a starting guide, these days usually qualify:

- a move of roughly 70% of the stock's daily limit or more (about 7% on the main board, 14% on
  ChiNext/STAR), or a limit-up / limit-down;
- turnover well above its recent norm;
- a break above or below a known level;
- a sharp divergence from its sector.

These guides are not a gate, and will be tuned after real runs. Record a day that meets none of
them if you judge it notable, and say why in `note`. Skip a day that meets one but tells you nothing
about this stock — for example, a move that simply matched the whole market's.

**Write the entry a few days after the fact.** `follow_through` cannot be known on the day itself,
and the file is append-only, so waiting until follow-through is visible avoids editing entries
later. History is never time-critical.

## Creating a profile for a new stock

1. Create the folder using the bare code.
2. Copy the three templates in, keeping every field.
3. Fill what the research actually established; leave the rest `unknown`.
4. Set `created_at` and the `last_updated` / `as_of` fields.
5. List what is still unfilled in `sections_needing_refresh`.

Baseline creation is a research task in its own right. If a daily run cannot do it properly, create
the profile with what is known and flag the gaps — do not let a thin profile later be mistaken for
a researched one.

## Staleness

Two independent triggers. Check **both**.

### 1. Time

| File | Default refresh |
|---|---|
| `financial_valuation_snapshot.json` | Each new 季报 / 半年报 / 年报; the valuation half re-checked monthly, or sooner after a large move |
| `stock_profile.json` | Its behavior fields (personality, key levels) every 2–4 weeks while actively tracked — these go stale fastest. The rest quarterly, or whenever the narrative changes |

`trading_history.jsonl` has no refresh cadence — it is append-only and never goes stale. Entries are
added when a notable day occurs, not on a schedule.

### 2. Events — standard triggers

These apply to **every** stock and do not need restating per file. Refresh regardless of date when:

- a new quarterly or annual report, or earnings guidance, is published;
- a major price rerating occurs;
- concept or theme exposure materially changes;
- a balance-sheet or shareholder risk event lands (减持, pledge, goodwill impairment, 问询函,
  ST warning);
- for market behavior specifically: a limit-up/limit-down day, a turnover jump well above norm, or
  a breakout/breakdown of a known level.

Each file's `refresh_triggers` list is for **exceptions only** — a trigger genuinely specific to
that stock ("reacts to 光伏 tariff announcements"). Do not copy the standard list into it.

### How triggers actually get checked

A trigger is a **checklist applied in reverse**, not a detector. The news that fires it is found
during the macro/sector phases, long before any profile is opened; by the time a stock is being
reviewed, that news is already known, and the question is "did anything today match?"

This means the burden sits on **selection**, not on the profile: a stock never selected for review
never gets its triggers checked. The morning program must select stocks whose sector, industry, or
tags were touched by the day's news — which it can do from `tracked_stocks.json` alone, without
opening any profile.

If a file is stale and there is no time to refresh it, say the data is stale, add the section to
`sections_needing_refresh`, and do not present old figures as current.

## Update discipline

- Update only the section that changed. Do not regenerate a profile to tidy it.
- Update the matching `last_updated` / `as_of` field whenever content changes.
- When a heavy file changes, update its summary in `stock_profile.json` so the cover page does not
  contradict the file beneath it.
- Append to `change_log` when a view materially changes, so the reasoning stays recoverable.

## Seeding history for a new stock

Populating a few months of notable days when a stock is first added is what makes the profile's
behavior verdicts possible at all — without it, "trading personality" is guesswork.

**Only do this if the price history is genuinely retrievable.** If it cannot be fetched, say so and
leave `trading_history.jsonl` empty rather than reconstructing it from memory. An invented history
would then be used to derive a personality verdict, which would be used to make predictions — a
fabrication at the base of that chain corrupts everything above it.
<!-- READ-CHECK: RC-PROFILES-R6L8 -->
