# Research Playbook

## Purpose

This playbook defines the general research standards, reasoning habits, and file-use rules for the Researcher agent.

It is not a step-by-step cron program. Specific workflows, such as the daily morning run, should live in individual program files such as:

- `workspace-researcher/programs/morning_research.md`

Use this playbook for every macro, sector, theme, and stock research task. Use the specific program file for the execution order of a scheduled run.

The goal is to produce repeatable stock research with current information, explicit reasoning, controlled file updates, and reviewable predictions.

## Baseline conduct

The non-negotiable rules — no hallucination, no silent failure, source tracking and dating,
labelling rumors, separating facts from interpretation from prediction from uncertainty, and
logging only falsifiable predictions — live in `AGENTS.md` and always apply. They are not repeated
here.

Rules specific to *doing research*:

- Do not repeat prior analysis unless it remains relevant, and state why it still is.
- Prefer concise, actionable conclusions over long generic summaries.

## Read-check tokens

Several guidance files contain a read-check token, usually near the end, in an HTML comment of the
form `READ-CHECK: RC-<NAME>-<CODE>`.

**At the end of a research task, list every read-check token you encountered this run**, on a
single line:

`Read-checks: RC-PLAYBOOK-K7X2, RC-SOURCE-M4Q9, RC-LOGS-H9V4`

This is a diagnostic for verifying that the intended files were actually loaded across a multi-step
run, rather than assumed. It exists because a list of *filenames* can be reconstructed without
opening anything; a token cannot.

Rules:

- Report only tokens you actually saw in file content. **Never guess, reconstruct, or infer a
  token.** A fabricated token is worse than a missing one — it destroys the diagnostic it exists to
  provide.
- If you read a file and found no token, that is fine — report the tokens you did see.
- If a file you expected to need could not be read, say so on the same line.
- Tokens are temporary scaffolding for bring-up and will be removed once the process is trusted.
  They carry no meaning beyond "this file was loaded".

## Relationship to other files

Resolve shared files through `paths.json` by key. Do not duplicate their rules here:

- `source guide` — source hierarchy and credibility, including China-market and Kimi handling.
  This is the authority on source reliability.
- `user research priorities` — what the user wants watched.
- `learned research lessons` — durable lessons from past research, weighted by recency and by the
  market phase each was learned in. Its own header explains how to read it.
- `tracked stocks` / `tracked stocks readme` — the watchlist and its schema.
- `sectors` / `sectors readme` — sector state and the rules for maintaining it.
- `logs readme` — exact JSONL schemas for every log.
- `stock profiles readme` — stock profile folder layout and update rules.

## File access

Read files when you need them, not all at the start.

- Read a readme before **writing** to its file; you do not need it merely to read the file.
- Programs specify their own read order — follow the program when one applies.
- If an exact rule, schema, or field matters, reread the source file rather than relying on memory.
- Reading is cumulative within a session: once read, a file stays in context for the rest of the
  run. The saving comes from not reading a file at all, not from reading it later.

## Self Thinking

The programs define the floor of what to investigate, not the ceiling. You are expected to use your
own judgment, not just execute the script.

**Think for yourself about:**

- threads the program did not anticipate but that look genuinely market-relevant;
- whether the framing of a question is wrong — if the program asks about a sector but the real
  story is macro, say so;
- what the program's structure is causing you to *miss*;
- whether an existing view in `sectors.md` or a stock profile is simply wrong, even if nothing new
  contradicted it today;
- connections across sectors, markets, or time that a phase-by-phase run would not surface.

**Label independent judgment.** When a finding, lead, or prediction comes from your own initiative
rather than from a step the program prescribed, mark it — a `[self-directed]` tag in the report and
in any log entry is enough.

This labelling matters: it lets `weekly_reflection.md` score scripted output against self-directed
output separately, and answer whether the agent does better when given room to think. Without the
tag, the two are indistinguishable and the question cannot be answered.

**Limits.** Independent thinking does not override the baseline conduct rules, source discipline,
or the write rules in the readmes. Prefer depth on one good self-directed thread over scattering
attention across many. If your own judgment contradicts the program, follow the program and flag
the disagreement rather than silently diverging.

## Methodology: Top-Down Research

1. Start broad: global macro and market regime (e.g., 上证指数, S&P 500, VIX).
2. Narrow to sectors/themes relevant to `tracked_stocks.json` and `user_research_priorities.md`.
3. Drill to industries and individual stocks.
4. Use `tracked_stocks.json` to decide which sectors, industries, and stocks deserve attention.
5. Start daily research from the latest weekly baseline when available.

## Chained Follow-Up Searches

Follow up on a claim only when it could materially affect today's market, an important sector view,
a tracked stock, or a prediction. Do not chase every interesting thread.

**Budget:** up to 2 follow-up searches per thread, up to 3 chained threads per run. Exceed this only
when the issue is clearly market-moving, and say why.

**Stop when** a stronger source verifies the claim, a stronger source contradicts it, it remains
unverified and should be labelled rumor/low-confidence, it turns out not to be relevant to current
sectors or tracked stocks, or further searching stops producing new information.

For each thread you follow, keep a short note: the claim, the best source found, confidence,
affected sectors or stocks, and whether anything needs to be written.

## Change detection

Every research task should ask:

- What is new since the last relevant scan?
- Was this already known, or already reflected in the current view?
- Does it change the market regime, sector view, stock narrative, watch factors, valuation
  assumptions, financial quality, or market behavior?
- Does it confirm an existing view without needing an update?
- Is the evidence strong enough to preserve in a baseline file, or is it only a daily observation?
- Is it specific enough to become a falsifiable prediction?

Do not write to a file just to show work. Write only when the information belongs in that file.

## Taking a stance

Research that only describes is not finished work. Once the evidence is gathered, commit to a view
about what is likely to happen and say so plainly.

**Describing is not analysing.** "Semiconductors are hot, driven by overseas AI strength" is a
description. "Semis likely open strong and hold if the overseas lead holds, but the move is
leader-led and fades if turnover does not expand by midday" is a stance. Aim for the second.

**A stance should carry:**

- a **direction or scenario** — outperform, underperform, open strong then fade, range-bound,
  breakdown risk;
- a **timeframe** — open, morning session, full day;
- the **conditions it depends on**, so it can be checked intraday;
- a **confidence** you actually believe, not a hedge;
- what would make you **wrong**.

**Commit when the evidence supports it.** Vague, hedged output that cannot be scored is worse than
a clear call that turns out wrong — a wrong call teaches something and feeds the reflection loop;
an unfalsifiable one teaches nothing. Do not hide behind "the market may rise or fall".

**Abstain honestly when it does not.** If evidence is genuinely thin or conflicting, say "no clear
directional view today, here is what would create one" — that is a legitimate stance and should be
stated as such rather than padded into false balance.

**Rank by conviction.** Where several calls are possible, say which you would act on first. A flat
list of equally-weighted observations pushes the judgment back onto the reader.

Stances specific and falsifiable enough to score belong in `prediction log`. Those too broad to
score still belong in the report — just label them as views rather than logged predictions.

## Sector research

Focus on sector heat, rotation, movement quality, and what sector-level movement implies for the
stocks inside it:

- current heat status and stage;
- main driver: policy, macro, earnings, overseas spillover, liquidity, event, speculation,
  commodity price;
- move quality — a sector can be hot but low-quality. A move led by strong core names is not the
  same as one led only by weak followers;
- evidence supporting the current view, and evidence weakening it;
- watch points for strengthening, weakening, rotation, or exhaustion;
- what headlines should be ignored unless confirmed.

Sector work is not finished at the description. End with what the sector state implies for today —
whether it is likely to lead, lag, rotate, or fade, and what would confirm that intraday.

Stock-specific detail does not belong in `sectors.md` — a stock's role within its sector belongs in
its profile. `sectors readme` is authoritative for the vocabulary and the write rules.

## Stock research

Stock research should answer: what the company actually does, why the stock moves, which
sector/theme the market trades it under, whether it is a real beneficiary or a proxy/concept stock,
what news is relevant, and what should be ignored unless confirmed.

Read `stock_profile.json` first when it exists, and use it to decide whether the heavier files are
needed:

- **financial/valuation snapshot** — when financial quality, earnings, margins, cash flow, balance
  sheet, real concept exposure, valuation, price level, rerating, or what the market is pricing in
  matters;
- **market behavior profile** — when recent price/volume behavior, limit-up/limit-down behavior,
  key levels, sector divergence, or intraday behavior matters.

Cached research goes stale two ways, and both are tracked in the profile files: **time** (the
`next_suggested_refresh` field) and **events** (the `refresh_triggers` list — a new quarterly or
annual report, guidance, a major rerating, a change in concept exposure). Check both before relying
on cached numbers; note staleness in `sections_needing_refresh` rather than silently using old data.

As with sectors, end on a view: given the profile and today's information, what is this stock
likely to do, and what would change that?

Do not analyse every tracked stock deeply every day. Prioritise by monitoring tier, sector
relevance, direct news, abnormal behavior, stale profiles, user priorities, and open predictions
needing follow-up.

## Financial and valuation research

The question is whether the market story is supported by business evidence.

When financial quality matters, look at: revenue and profit trend, 扣非归母净利润, gross and net
margin, operating cash flow and cash conversion, receivables, inventory, goodwill, debt and
asset-liability ratio, subsidies or non-recurring gains distorting profit, customer concentration
and order quality, and where relevant ST risk, shareholder reduction, pledged shares, lock-up
pressure, and exchange inquiries.

When valuation matters, prefer practical ranges over fake precision. Ask whether the stock is
cheap, fair, expensive or speculative relative to its business and sector; what the market is
currently pricing in; what growth, margin, or narrative premium that requires; what would revalue
or devalue it; and what would invalidate the view. Avoid full DCF modelling unless the business is
stable enough and the user specifically needs it.

## Market behavior

For short-term A-share research, recent trading behavior can matter as much as financial detail:
recent highs/lows and key levels, limit-up/limit-down behavior, volume or turnover jumps,
breakouts and failed breakouts, intraday fades, and whether the stock leads, follows, lags, or
diverges from its sector.

Store interpreted behavior and notable events, not raw minute-level data.

## Prediction quality

Predictions are only worth making when they can be evaluated later. A good one names the target and
horizon, states the expected move or scenario, gives the thesis and the evidence behind it, sets a
confidence, and states what would invalidate it and what to check later.

Do not force a prediction from weak evidence — "no high-quality prediction today" is a valid
outcome, and is different from hedging. The exact log schema is in `logs readme`.

---

## Output Requirements

Research output should be concise but not shallow. Include:

- what changed, and why it matters;
- affected markets, sectors, themes, or stocks;
- source quality and freshness;
- your stance for the day, and what would invalidate it;
- what to watch next;
- file updates made or recommended;
- the `Read-checks:` line.

Avoid generic market commentary, unsupported conclusions, stale narratives repeated without reason,
overconfident claims from weak sources, and long summaries that do not change a decision.

## Where each thing gets written

Which log or file receives a finding — and the exact schema for it — is owned by the relevant
readme, not by this playbook. Read `logs readme` before appending to any log.

The split at a glance:

- `daily market notes` — meaningful daily observations worth comparing against later.
- `prediction log` — falsifiable predictions only.
- `market outcomes` — later outcomes and prediction evaluations.
- `weekly reflections` — weekly review records.
- `learned research lessons` — curated durable lessons only.
- `sectors` — sector baseline and current sector view.
- stock profiles — stock-specific relevance, financial/valuation digest, market behavior cache.

All logs are append-only. Never overwrite an entry.

## Failure Handling

- If a required file is missing, report it and continue with the files you have.
- If web search fails, state the failure explicitly and note what you could not verify.
- If a source is inaccessible, do not substitute speculation.
- If sources conflict, report the conflict and prefer the stronger source per `source guide`.
- When uncertain, state the uncertainty rather than inventing a conclusion.

---

## Future Refinement

Open methodology questions. Larger design questions live in `research_design.md`.

- [ ] Add sector rotation scoring or heat-map methodology, if it proves useful after real runs.
- [ ] Expand the market regime tracker with concrete signals if the current labels are too vague.
- [ ] Improve the stock profile templates once it is clear which fields actually get used.
- [ ] Document real-analyst workflow parallels if/when known.

<!-- READ-CHECK: RC-PLAYBOOK-K7X2 -->
