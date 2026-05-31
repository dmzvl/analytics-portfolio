# NYC 311 Service Request Volume Forecasting — Q1 2024

## Business Question

How many 311 service requests should New York City expect in Q1 2024, and what minimum staffing level does that imply for service operations?

## Key Finding

The model forecasts **728,856 total requests in Q1 2024** (80% CI: [624,533 – 832,362]), implying a baseline of **164 agents** at 50 requests/agent/day, with surge capacity of 187 at the upper bound of the interval.

**Honest constraint:** Prophet did not outperform a naive baseline (yesterday's volume) on the Q4 2023 holdout period — Prophet MAE ~1,117 vs. naive MAE ~1,018 requests/day. The model's value is not point-estimate precision; it is the decomposed seasonal structure and quantified uncertainty interval that a naive forecast cannot provide. The interval width (~208,000 requests) reflects real uncertainty and should be treated as directional.

## Methodology

Three years of NYC 311 daily request volume (2021–2023, N=1,095 days) were sourced from the NYC Open Data SODA API using server-side aggregation. A Prophet time-series model was trained with weekly and annual seasonality components. Two weather events — Hurricane Ida remnants (September 2021, +73% above mean) and a major NYC rainfall event (September 2023, +105% above mean) — were passed as named holiday regressors to prevent contamination of the September seasonal baseline.

A 90-day temporal holdout (Q4 2023) was used for evaluation. ARIMA(1,1,1) was tested as an alternative and rejected: it handles only a single seasonal period natively, requires manual stationarity handling, and provides no mechanism for isolating named event effects. A quantitative comparison is documented in the Appendix. Changepoint sensitivity was tested across three values (0.01, 0.05, 0.15); results were stable.

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

Install dependencies:
```bash
pip install prophet pandas numpy matplotlib seaborn scikit-learn statsmodels requests
```

## How to Run

1. Clone the repository
2. Install dependencies (see above)
3. Open `nyc_311_forecasting_portfolio.ipynb` in Jupyter
4. Run all cells top to bottom — data loads directly from the NYC Open Data API

No local data files required. The notebook pulls pre-aggregated daily volume from the public SODA API on execution.

## Limitations

1. **Seasonal stability assumption:** Prophet learns seasonal patterns from 2021–2023 and assumes they repeat in Q1 2024. A volume growth trend not visible in the training window would cause the forecast to underpredict — which is what occurred in the Q4 2023 test period.

2. **Narrow uncertainty intervals:** Coverage on the test period was below the 80% target, suggesting the model's confidence intervals may understate actual volatility. Treat the interval as directional guidance rather than a precise probability statement.

3. **Illustrative staffing assumption:** The 50 requests/agent/day throughput figure is a placeholder. Substitute actual capacity data before using this analysis for operational planning.

---
