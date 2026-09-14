# Source Guide

This guide defines a simple source hierarchy for financial research. Use it to judge credibility and decide how much weight to give a piece of information.

## Source Levels

### A-level — Primary / Official

- Official company disclosures (annual reports, interim reports, announcements)
- Exchange filings and regulatory submissions
- Government macro/policy releases (e.g., PBOC, CSRC, State Council)
- Official company press releases

### B-level — Reputable Financial Media and Data Providers

- Major financial media (e.g., Reuters, Bloomberg, Financial Times, Caixin, 财新)
- Reputable data providers (e.g., Wind, Choice, Bloomberg Terminal data)
- Brokerage research or summaries from recognized firms

### C-level — Aggregators and Market Data Summaries

- Financial portals (e.g., Sina Finance, East Money / 东方财富, Snowball / 雪球)
- Market-data summaries and screeners
- News aggregators

### D-level — Social and Unverified

- Social media, forums, chat groups
- Unsourced rumors
- Influencer commentary without disclosed data or methodology

## Usage Rules

- Prefer A/B sources for company-specific factual claims.
- Use C/D sources mainly for discovery, sentiment, or leads.
- Do not treat a C/D claim as fact unless it is verified by an A/B source.
- Do not convert rumor into fact.
- Record source date/freshness when using web research.
- If a source is behind a paywall or inaccessible, report that explicitly rather than omitting the citation.

## What the hierarchy is for

The A/B/C/D levels govern **contested or consequential claims** — what a company earns, what a
policy says, whether an order was actually signed.

They are not meant to make routine market data expensive. Yesterday's close, turnover, sector index
moves, and limit-up counts are fine from a C-level portal; you do not need an exchange filing to
know what a stock did yesterday. Escalate to A-level when the claim is disputed, surprising,
company-specific, or about to be written into a baseline file.

## China-Market Sources

For A-share company facts, Chinese-language official sources are both the most authoritative and
the most practically accessible. Prefer them over English coverage of the same event.

### A-level — official disclosure and regulators

- **巨潮资讯网 / CNINFO** (cninfo.com.cn) — the CSRC-designated disclosure portal, carrying
  announcements, annual and interim reports for both Shanghai and Shenzhen listings. First stop for
  anything a listed company has formally disclosed.
- **Exchanges** — 上交所 / SSE (sse.com.cn), 深交所 / SZSE (szse.cn), 北交所 / BSE (bse.cn). Each
  also hosts filings for its own listings, plus inquiry letters (问询函), suspensions, and
  disciplinary actions.
- **Regulators and ministries** — 证监会 / CSRC, 中国人民银行 / PBOC, 国家统计局 / NBS,
  国务院 / State Council, 发改委 / NDRC, 工信部 / MIIT. Policy releases and official macro data.
- Company investor-relations pages and official announcements.

Hong Kong listings: **HKEXnews / 披露易** (hkexnews.hk) is the equivalent disclosure source.

### B-level — reputable financial media and data

- The CSRC-designated securities newspapers: 中国证券报, 上海证券报, 证券时报, 证券日报.
- 财新 / Caixin, 第一财经 / Yicai.
- International: Reuters, Bloomberg, Financial Times — often better for global macro and for
  overseas reaction to Chinese policy.
- Institutional data: Wind, Choice.
- **券商研报 / brokerage research** — reliable on assembled facts and useful for industry
  background, but structurally **sell-side**: ratings skew to buy, sell ratings are rare, and the
  bank may have a banking relationship with the issuer. Treat the data as B-level and the
  conclusion as an opinion with an interest behind it. Never adopt a price target as a fact.

### C-level — portals and aggregators

东方财富 / East Money, 新浪财经 / Sina Finance, 同花顺, 雪球 / Xueqiu.

Fast and practical for discovery, quotes, sector heat, turnover, and what the market is talking
about — and fully adequate for routine market data. Not authoritative for company-specific factual
claims; verify those against A-level. Xueqiu mixes user commentary with data — treat the commentary
as D-level.

### D-level — social and unverified

股吧, forums, WeChat groups, screenshots, influencer commentary, and unattributed 小作文.
Discovery and sentiment only. These can tell you *what the market believes*, which is genuinely
useful — just never confuse it with what is true.

### Mandarin vs. English

- Use the **Chinese original** for anything official. English coverage of a Chinese filing or policy
  release is a translation, and drift on policy wording is common.
- Keep the original term alongside a translation where the wording carries weight (扣非归母净利润,
  问询函, 减持, 停牌).
- English/international sources are appropriate for global macro, overseas moves, and foreign
  reaction — but do not assume every Western source is reachable.

## Kimi / Moonshot search

Kimi search is a **discovery layer, not a source.**

- A synthesised answer is a lead. Identify the underlying source before treating any claim as fact,
  and cite that source rather than Kimi.
- Treat it as usable evidence only when it supplies a **source name, a date, and ideally a link**.
  Without those, label it unverified.
- A confident summary can rest on a weak source. Grade the source underneath, not the fluency of
  the summary.
- Prefer Chinese-language queries for A-share topics.
- If search is unavailable, blocked, or returns nothing usable, **say so explicitly**. Never fill
  the gap from prior knowledge and present it as current — in a market that moves daily, stale
  recall presented as fresh is the most damaging failure mode available.

Kimi's actual retrieval behaviour on this setup is **not yet established** — which sources it
reaches, what it paywalls out on, how current its results run. As that becomes clear through real
runs, record it in `TOOLS.md` (stable facts) or `MEMORY.md` (evolving observations) rather than
assuming it here.

## Data you cannot verify

Do not fabricate a figure to complete a table. If a number cannot be found, write `unknown` and say
where you looked. A missing number is a normal research outcome; an invented one silently corrupts
every downstream file that copies it.

<!-- READ-CHECK: RC-SOURCE-M4Q9 -->
