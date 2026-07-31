# Sectors File Guide

## Purpose

`sectors.md` stores compact, trading-relevant sector context so the Researcher can understand how broad sector movement may affect stocks within that sector. It should provide information about the current status of the sector and predictions as to how it may move next.

The goal is to track:

* whether a sector/theme is heating up, hot, cooling, exhausted, ignored, or reactivating;
* what is currently driving the sector: policy, macro news, overseas spillover, earnings, liquidity, speculation, supply-chain events, commodities, geopolitics, or narrative hype;
* whether the move is broad and healthy, leader-led, follower-led, narrow, speculative, or weak;
* what evidence supports or weakens the current sector view;
* what signs suggest the sector is strengthening, weakening, rotating, or ending;
* what kinds of news matter for this sector;
* what kinds of headlines should be ignored unless confirmed;
* how sector-level movement should shape expectations for stocks inside the sector.

This file is not a daily news dump. Daily events belong in `daily_market_notes.jsonl`.

`sectors.md` should store the current sector baseline, heat status, narrative, movement quality, evidence for/against the current view, and watch points.

## Relationship to other files

* Use `daily_market_notes.jsonl` for daily market events and one-day observations.
* Use `sectors.md` for sector baseline, current heat status, current sector view, move quality, and sector-level watch points.
* Use stock profiles for stock-specific relevance, stock-specific sector role, and whether a stock is a leader, follower, proxy, or real beneficiary.
* Use `prediction_log.jsonl` for specific falsifiable forecasts.

## Update rules

Update `sectors.md` only when the sector-level picture changes.

Update when one of these changes:

1. Sector heat status.
2. Current sector view.
3. Main driver.
4. Move quality.
5. Evidence supporting or weakening the current view.
6. Important watch points.
7. Important policy, macro, geopolitical, commodity, or overseas-spillover sensitivity.
8. A new sector/theme becomes relevant to current market research.

Examples of how world events might affect their relevant sectors:
- Oil/shipping risk rises due to Middle East escalation → update 航运 / 石油 / 黄金 if relevant.
- U.S. export-control news affects semiconductors → update 半导体.
- Japan market spillover becomes relevant to A-share risk appetite → update broad market / export-sensitive sectors if represented.

Do not update `sectors.md` for:

* routine headlines;
* weak rumors;
* repeated unchanged information;
* isolated company news with no sector-level implication;
* minor price movement that does not change the sector view.


If the information is useful but does not change the sector baseline, write it to `daily_market_notes.jsonl` instead.

## Adding a new sector

If the relevant sector is missing entirely or has not been filled out, it must be researched from scratch.

## LLM reasoning rule

When using `sectors.md`, the Researcher should not mechanically trust the existing sector label.

For each relevant sector, ask:

1. Is the current sector view still accurate?
2. Does new information support the current view?
3. Does new information weaken or complicate the current view?
4. Is the sector becoming hotter, colder, broader, narrower, more speculative, or more fundamentally supported?
5. Does the sector deserve more, less, or the same amount of attention today?

Only update the sector entry when the answer changes the sector-level view.

## Sector entry format

Each sector should use this structure:

```markdown
## <Sector / Theme Name>

### Current sector state
- Current view:
- Heat status:
- Stage:
- Main driver:
- Move quality:
- Near-term expectation:
- Confidence:
- Last reviewed:

### Why this sector moves
-
-
-

### Evidence supporting current view
- (YYYY/MM/DD + Evidence)
-
-

### Evidence against or weakening current view
- (YYYY/MM/DD + Evidence)
-
-

### Watch points
-
-
-


```

## Field guidance

### Heat status

Use one of these when possible:

* `ignored`
* `warming`
* `hot`
* `cooling`
* `exhausted`
* `reactivating`
* `unclear`

This should describe current trading relevance, not long-term industry importance.

### Stage

Use one of these when possible:

* `early`
* `middle`
* `late`
* `post-hype`
* `unclear`

This helps avoid treating a crowded mature theme the same as an emerging one.

### Main driver

Use concise labels such as:

* `policy`
* `macro`
* `earnings`
* `overseas spillover`
* `liquidity`
* `geopolitics`
* `commodity price`
* `supply-chain event`
* `theme speculation`
* `company announcements`
* `unclear`

Multiple drivers are allowed, but avoid listing everything.

### Move quality

Use one of these when possible:

* `broad`
* `leader-led`
* `follower-led`
* `speculative`
* `weak`
* `mixed`
* `unclear`
* `unsure`

This matters because a sector can be hot but low-quality. A move led by strong core names is different from a move led only by weak followers.

### Current view

The current view should be a thesis about the sector’s present market condition.

Good examples:

* “Semiconductors remain hot, but leadership quality matters. The move is strongest when policy support and overseas AI hardware strength align.”
* “Gold-related names are mainly risk-aversion trades. They should be watched when geopolitical risk, USD weakness, or real-rate expectations shift.”
* “Shipping is event-sensitive rather than structurally hot. Moves need confirmation from freight rates, route disruption, or oil/shipping news.”

Avoid generic statements like:

* “This sector is important.”
* “Pay attention to news.”
* “The sector may rise or fall.”

### Evidence supporting current view

Use this for observed evidence that makes the current view more credible.

Examples:

* Sector leaders are outperforming.
* Turnover is expanding.
* Overseas linked stocks are strong.
* Official policy supports the theme.
* Earnings, orders, or industry data support the narrative.
* The sector remains strong despite a weak broad market.

### Evidence against or weakening current view

Use this for observed evidence that makes the current view less reliable.

Examples:

* Leaders lag while low-quality followers rise.
* The sector opens strong but fades intraday.
* Turnover dries up during rebounds.
* Companies clarify limited exposure.
* Earnings or orders fail to support the market story.
* Money rotates into another theme.

### Watch points

Use this for things to monitor.

Examples:

* leader stocks versus followers;
* turnover expansion or contraction;
* intraday fades;
* sector index strength versus broad market;
* policy/news flow;
* overseas spillover;
* earnings or order confirmation;
* signs of theme exhaustion.

### Review triggers

This section is optional.

Use it only for concrete conditions that would force the sector view to be reviewed. Do not fill it with generic statements.

Good examples:

* “Review if sector leaders lag for multiple sessions while low-quality followers dominate.”
* “Review if official policy contradicts current support expectations.”
* “Review if companies clarify limited exposure to the current theme.”
* “Review if turnover contracts during attempted rebounds.”

If there are no concrete triggers, write:

```markdown
### Review triggers
- None specific yet.
```

## Writing style

Write for trading relevance.

Prefer:

* concise bullets;
* current market interpretation;
* observed evidence;
* clear uncertainty labels;
* practical watch points.

Avoid:

* long essays;
* generic industry descriptions;
* individual stock details;
* copying headlines without interpretation;
* repeating unchanged content;
* pretending weak evidence is strong.

Unknown fields may be left as `unknown`, `unclear`, or `TODO`.

## Bloat control

`sectors.md` should stay compact enough for routine research.

To control bloat:

* Keep each sector entry short.
* Replace stale bullets instead of endlessly appending.
* Put daily observations in `daily_market_notes.jsonl`.
* Keep `Recent changes` short.
* Do not include stock-specific details.
* Do not add sectors that are not relevant to user priorities, tracked sectors, or current market movement.

## Update discipline

Before editing `sectors.md`, ask:

1. Is this a sector-level change or just a daily event?
2. Does it change the current sector view?
3. Does it change heat status, driver, move quality, or watch points?
4. Is the evidence strong enough to preserve in the sector baseline?
5. Would this be better stored in `daily_market_notes.jsonl`?

Only edit `sectors.md` when it is a durable sector update.
