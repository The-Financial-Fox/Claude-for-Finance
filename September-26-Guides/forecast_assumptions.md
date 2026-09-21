# Forecast Assumptions — FY2025 Q4 → FY2026 Revenue Forecast

**Prepared for:** FP&A forecasting demo (Claude ML forecast)
**Forecast horizon:** October 2025 – December 2026 (15 months)
**Base data:** Cleaned monthly actuals, Jan 2023 – Sep 2025 (33 months), by business unit × region
**Owner:** FP&A — update before every forecast refresh; Claude should treat this file as ground truth and flag if the cleaned data contradicts it.

---

## 1. Known one-off events already baked into history (do NOT extrapolate these as trend)

| Event | Business unit | Region | Period | Effect | Forecast treatment |
|---|---|---|---|---|---|
| Large enterprise renewal | Enterprise SaaS | North America | Nov 2024 | +$210K one-time spike | Exclude from trend/seasonality fitting; do not repeat in Nov 2025 unless Sales confirms a similar renewal is contracted |
| Supply chain disruption | Hardware | EMEA | Mar–Apr 2024 | Revenue dropped ~45% | Treat as a temporary shock, not a new baseline; Hardware EMEA should return to trend by the period after |
| New-market launch ramp | Marketplace | APAC | Jun 2024 onward | +35% step-change, sustained | This IS a genuine step-change in the base — carry the new (higher) level forward, don't average it away with pre-launch months |

## 2. Growth assumptions by business unit (management's directional view)

| Business unit | Recent trend (observed) | FY2026 assumption | Rationale |
|---|---|---|---|
| Enterprise SaaS | ~1.8%/month underlying growth | Continue at ~1.5–2.0%/month | Pipeline coverage remains healthy (~3x); no major macro headwind assumed |
| SMB SaaS | ~1.2%/month | Continue at ~1.0–1.3%/month | Stable segment; watch for price-sensitivity if macro assumption below worsens |
| Professional Services | ~0.4%/month (roughly flat) | Flat to slightly up | Capacity-constrained; growth requires headcount additions not yet approved |
| Marketplace | ~2.1%/month, plus APAC step-change | Continue at ~2.0%/month off the new (post-launch) base | APAC ramp assumed to mature, not accelerate further, through 2026 |
| Hardware | ~-0.6%/month (declining) | Continue gradual decline, ~-0.5% to -0.8%/month | Line is being deprioritized per FY2026 product roadmap; no new SKUs assumed |

## 3. Seasonality

- **Q4 is the strongest quarter** across all business units (October–December), driven by enterprise budget cycles. December typically runs ~20–25% above the annual monthly average.
- **Q3 (Jul–Aug) is soft**, particularly in **EMEA**, due to summer holiday slowdown — assume an additional ~10–15% dip in EMEA specifically in July/August on top of the general Q3 softness.
- **January–February** are the softest months overall as budgets reset.
- The model should learn seasonality from the 33 months of history (Jan 2023–Sep 2025 spans nearly 3 full cycles) rather than have it hand-coded — but results should be sanity-checked against the pattern above.

## 4. Macro / external assumptions

- **FX**: Assume rates held flat at the Sep 2025 average for all forecast periods (no FX-driven revenue swings modeled). If Treasury/Finance provides a hedged FX view, this should override.
- **No recession scenario assumed** in the base case. A downside scenario (see Section 6) exists separately for board sensitivity discussion.
- **No planned M&A** is included in the base forecast.
- **Pricing**: assume no list price changes in the forecast window; any pricing action agreed by GTM should be layered on top of the base model output, not embedded in it.

## 5. Data quality guardrails for the model

- **Outliers**: Any single month's revenue for a business unit × region combination that exceeds 5x the trailing 6-month average should be flagged as a probable data error and excluded from trend-fitting (see the $99,999,999 entries in the raw exports — these are fat-finger errors, not real revenue, and must not enter the model).
- **Unmapped cost centers**: Rows with an unrecognized `business_unit` label (e.g., "Unmapped-CC-4471") should be excluded from the forecast and flagged separately for Finance to investigate and reclassify — do not guess which business unit they belong to.
- **Missing values**: Missing `pipeline_created` or `deals_closed` should not block forecasting `revenue_actual` — treat each metric's completeness independently.
- **Duplicate rows**: Exact duplicate (month, business_unit, region) rows are export artifacts, not real double-counted revenue — deduplicate before fitting.

## 6. Sensitivity / scenario request

In addition to the base case, produce a **downside scenario** at -8% to underlying growth rates across all business units from Jan 2026 onward (a mild demand-slowdown scenario), for board risk discussion. Do not blend this into the base forecast — present both.

## 7. What "good" looks like for this forecast

- Monthly granularity, by business unit and region, for the full horizon.
- Confidence range (e.g., P10/P50/P90 or a simple ± band) shown alongside the point forecast — FP&A needs to communicate uncertainty, not just a single number.
- A short written explanation of what's driving the forecast shape (trend vs. seasonality vs. one-offs), in plain English suitable for a leadership audience.
- Clear flagging of any month where the model's output looks inconsistent with Sections 1–4 above, so a human can review before it goes in the deck.
