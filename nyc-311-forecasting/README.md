# NYC 311 Service Request Volume Forecasting — Q1 2024

## Business Question

How many 311 service requests should New York City expect in Q1 2024, and what minimum staffing level does that imply for service operations?

## Key Finding

The baseline Prophet model forecasts **728,856 total requests in Q1 2024** (80% CI: [624,533 – 832,362]), implying **164 agents** at 50 requests/agent/day with surge capacity of 187 at the upper bound.

**Two findings that must accompany this number:**

All four model specifications tested — the baseline plus three targeted tuning iterations (higher changepoint flexibility, shortened training window, and a combined specification) — were outperformed by a naive baseline (yesterday's volume, MAE ~1,018 vs. Prophet's best of 1,095 on the Q4 2023 test period). The underestimation of Q4 2023 actuals reflects genuine volume growth not visible in the training window, not a correctable tuning problem. The tuning null result is documented explicitly as an analytical finding.

The model's value is not point-estimate precision on a single test quarter. It is the decomposed seasonal structure that explains *why* volume moves, the 80% uncertainty interval that quantifies planning risk, and the decision framework that connects the forecast to a staffing trigger.

## Methodology

Three years of NYC Open Data daily 311 volume (2021–2023, N=1,095 days) were sourced via the SODA API using server-side aggregation. Prophet was selected over ARIMA/SARIMA because the series exhibits two overlapping seasonality layers (weekly and annual) that ARIMA handles with a single seasonal period, and because Prophet's `holidays` parameter cleanly isolates two identified weather event spikes — Hurricane Ida remnants (September 2021, +73% above mean) and a major NYC rainfall event (September 2023, +105% above mean) — that would otherwise contaminate the September seasonal baseline.

A 90-day temporal holdout (Q4 2023) was used for evaluation, matching the quarterly forecast horizon. Three tuning dimensions were tested against the baseline: changepoint flexibility (scale=0.05 vs. 0.30), training window length (2021–2023 vs. 2022–2023 only), and a combined specification. All three performed worse than the default baseline, and the correct diagnosis — a data currency problem, not a tuning problem — is documented in Section 3.

## Technical Requirements

```
python >= 3.9
prophet
pandas
numpy
matplotlib
seaborn
scikit-learn
statsmodels
requests
```

Install:
```bash
pip install prophet pandas numpy matplotlib seaborn scikit-learn statsmodels requests
```

## How to Run

1. Clone the repository
2. Install dependencies (see above)
3. Open `nyc_311_forecasting_portfolio.ipynb` in Jupyter
4. Run all cells top to bottom — data loads directly from the NYC Open Data SODA API

No local data files required. The notebook pulls pre-aggregated daily volume from the public API on execution.

## Limitations

1. **Data currency:** The Q4 2023 test period showed genuine volume growth not visible in the 2021–2023 training window. The correct intervention is a quarterly retraining protocol — not a hyperparameter change. No specification adjustment can fix a model trained before the growth became observable.

2. **Narrow uncertainty intervals:** Coverage on the Q4 2023 test period was 66% against an 80% target. The model's confidence intervals understate actual series volatility. Treat the interval as a directional planning range, not a statistically precise probability statement.

3. **Illustrative staffing assumption:** The 50 requests/agent/day throughput figure is a placeholder. Substitute actual capacity data before using this analysis for operational planning. The directional implication is valid; operational precision requires real throughput data.

---

*Part of an analytics leadership portfolio demonstrating applied forecasting, honest model evaluation, null-result documentation, and translation of statistical output to operational decision frameworks.*
