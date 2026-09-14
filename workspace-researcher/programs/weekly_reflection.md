# Weekly Reflection

## Purpose

The weekly self-improvement pass. Reviews the week's predictions against what actually happened,
records the raw review, and promotes anything durable into curated lessons.

This program **reflects**; it does not research. The weekly deep-research run that sets the
baseline for the coming week is `weekly_research.md`, and it is a separate program.

Runs once per week, after the last trading session of the week.

## Inputs

Resolve through `paths.json`:

- `daily market notes` — the week's entries
- `prediction log` — predictions made during the week
- `market outcomes` — outcomes recorded by `daily_reflection.md`
- `learned research lessons` — the current curated set, to avoid re-learning what is already known

## Steps

1. Read the week's entries from `daily market notes`.
2. Read the week's predictions and their matching outcomes. Predictions with no recorded outcome
   are themselves a finding — note them rather than quietly skipping them.
3. Assess: what worked, what failed, and what was merely luck. A correct call for the wrong reason
   is a failure, not a success.
4. Look for patterns across the week rather than judging each prediction in isolation. One miss is
   noise; the same kind of miss three times is a lesson.
5. Write the raw weekly summary to `weekly reflections` — read `logs readme` first for the schema.
6. Promote durable lessons into `learned research lessons`, following the curation rules below.

## Curating `learned_research_lessons.md`

`weekly reflections` is the raw record. `learned research lessons` is the curated set the morning
run reads every day. Most reflections should **not** graduate. Read the file's own header for the
entry format before appending.

**A reflection graduates into a lesson when:**

- it has recurred, or it cost something significant once;
- it is stated as a rule that would change a future decision, not as a description of what
  happened;
- it is not already covered by an existing lesson. If it refines one, edit that lesson instead of
  adding a near-duplicate.

**It does not graduate when:**

- it is a one-off observation with no clear repeat mechanism;
- it is a market fact rather than a lesson about how to research — market facts belong in
  `daily market notes` or `sectors.md`;
- it restates something already in the playbook or a readme.

**Tag the market phase.** Record the phase the lesson was learned in (`bull`, `bear`,
`consolidation/震荡`, `policy-driven`, `liquidity-driven`, `theme-driven`, `unclear`). A lesson
learned in one phase may not hold in another, and the morning run relies on this tag to weight it.

**Retiring a lesson.** When later evidence contradicts a lesson, mark it `still valid?: no` with the
contradicting evidence attached. Do not delete it — a struck lesson records something already
tried, which prevents re-learning it. Only delete a lesson that was factually wrong when written.

**Keep it readable.** The morning run reads this file every day. If it grows past roughly 30 active
lessons, consolidate overlapping ones rather than letting it sprawl. Struck lessons can be grouped
at the bottom.

## Output

A short summary for the user:

- what was predicted versus what happened;
- hit rate and, more usefully, the *character* of the misses;
- lessons promoted this week, and lessons struck;
- anything the coming week's `weekly_research.md` run should specifically investigate.

Separate facts, interpretation, and uncertainty as in any other output. If the week produced no
durable lesson, say so — do not manufacture one to show progress.
