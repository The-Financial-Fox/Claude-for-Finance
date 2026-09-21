# FP&A Forecasting Demo — Prompt Script

Four stages, one continuous Claude conversation. Copy-paste each prompt in order.
Setup tags reference the model/mode/output guide from the full interface guide:
👉 https://claude.ai/artifact/2P94jEA18SD9FjMrKimhUp

**Before you start:** attach the 3 files from `raw_data_folder/` and the
`forecast_assumptions.md` file to the conversation (drag them in, or use the
`+` menu → Add files or photos).

---

## Stage 1 — Ingest & clean

**Setup:** `Sonnet 5` · `Medium effort` · `Manual mode` · `Output: Artifact (let Claude pick)`

> I'm dropping in three exports from different systems that all describe the
> same monthly revenue and pipeline data — a CRM export, a billing system
> extract, and an ad hoc regional upload. They were never designed to match
> each other.
>
> Please:
> 1. Load all three files and combine them into one table.
> 2. Standardize the date formats, business unit names, and region names —
>    they're inconsistent across and within files (typos, casing, trailing
>    spaces, mixed date formats).
> 3. Convert revenue figures that are stored as text with currency symbols
>    into numbers.
> 4. Remove exact duplicate rows.
> 5. Flag (don't silently delete) any row that looks like a data entry error —
>    for example a single month's revenue that's wildly out of line with
>    everything else for that business unit — and any row with a business
>    unit you don't recognize.
> 6. Give me a short data quality summary: how many rows you started with,
>    how many duplicates you removed, how many you flagged and why, and the
>    final row count.
>
> Save the cleaned dataset as a CSV I can use for forecasting next.

---

## Stage 2 — Sanity-check against assumptions

**Setup:** `Sonnet 5` · `Medium effort` · `Manual mode`

> Here's our forecast_assumptions.md file. Before we forecast, check the
> cleaned data against it:
> - Do the one-off events in Section 1 (the Nov 2024 renewal, the Hardware
>   EMEA disruption, the Marketplace APAC launch) actually show up in the
>   cleaned data the way this file describes them?
> - Does the seasonality in the actuals match what Section 3 describes?
> - Flag anything in the data that contradicts these assumptions before we
>   build the forecast on top of them.

---

## Stage 3 — ML forecast

**Setup:** `Opus 5` · `High effort` · `Manual mode` · `Output: Docs`

> Using the cleaned dataset and forecast_assumptions.md, build a monthly
> revenue forecast from October 2025 through December 2026, by business unit
> and region.
>
> Requirements:
> - Use an appropriate time-series / ML approach that can pick up trend and
>   seasonality from the ~33 months of history (e.g. a seasonal decomposition
>   or gradient-boosted/Prophet-style model — your call, but tell me what you
>   used and why).
> - Exclude the flagged data-error rows and the unmapped cost center from
>   training, per the assumptions file.
> - Treat the Marketplace APAC step-change as a new baseline, not a one-time
>   spike to smooth away, per Section 1.
> - Produce a base case AND the -8% downside scenario described in Section 6.
> - Show a confidence range (P10/P50/P90 or a ± band), not just a point
>   estimate.
> - Write up a short, plain-English explanation of what's driving the shape
>   of the forecast — how much is trend, how much is seasonality, and where
>   the one-off events matter — suitable for a leadership audience.
> - Flag any forecasted month that looks inconsistent with the assumptions
>   file so I can sanity-check it myself.
>
> Write this up as a doc with the methodology, the write-up, and a results
> table, and also give me the monthly forecast numbers as a CSV I can chart.

---

## Stage 4 — Dashboard

**Setup:** `Sonnet 5` · `Medium effort` · `Output: Artifact`

> Now build an executive dashboard from the cleaned actuals and the forecast
> output:
> - Headline KPI tiles: FY2026 forecasted revenue (base case), YoY growth %,
>   and the downside-scenario delta.
> - A monthly actuals + forecast line chart (with the confidence band shaded)
>   for total revenue, with a clear visual break between actuals and forecast.
> - A breakdown by business unit so I can see which lines are driving growth
>   vs. decline.
> - A callout box surfacing the flagged data-quality issues and the
>   assumption-conflict flags from Stage 2/3, so whoever reviews this can see
>   what was excluded or needs a second look.
> - Keep it to one screen, scannable in under a minute — this is for a
>   leadership review, not a working file.

---

## Optional Stage 5 — Make it recurring

**Setup:** `Sonnet 5` · `Auto mode` · `Scheduled`

> Turn this into a monthly scheduled task: on the first business day of each
> month, pull the latest exports from the same three sources, run the same
> cleaning and flagging logic, refresh the forecast, and update the
> dashboard. Only interrupt me if a new data quality flag shows up that we
> haven't seen the pattern of before — otherwise just have the refreshed
> dashboard ready.
