# Multivariate Time Series Forecasting using Python

VAR (Vector Autoregression) model for forecasting multiple stock tickers simultaneously, with stationarity testing, differencing, and TimescaleDB integration.

---

## Problem Statement

Stock prices don't move in isolation — Apple, Google, and Microsoft are correlated. Univariate models (ARIMA) ignore these cross-series relationships. How do you forecast multiple related time series together, capturing the interdependencies between them?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Model** | VAR (Vector Autoregression) | Models each series as a function of its own lags AND other series' lags — captures cross-correlations |
| **Stationarity** | ADF test + first differencing | VAR requires stationary input; differencing removes trends while preserving relationships |
| **Lag selection** | `maxlags=2` | Keeps the model simple; more lags would need more data to avoid overfitting |
| **Database** | TimescaleDB (PostgreSQL extension) | Purpose-built for time series; SQL-compatible, supports continuous aggregates |
| **Visualization** | Matplotlib with actual vs. forecast overlay | Direct comparison makes forecast quality immediately visible |

---

## What I Learned

- **VAR catches cross-series signals that univariate models miss.** When Apple's price drops, Google often follows. VAR models this lag structure explicitly — the forecast incorporated Apple's movement into Google's prediction.
- **Stationarity testing is not optional.** Running VAR on non-stationary data produces spurious correlations. The ADF test (p-value < 0.05 threshold) is a quick gate that prevents garbage-in-garbage-out.
- **Reverse differencing introduces cumulative error.** Forecasts are made in differenced space and cumsum'd back. Each step accumulates the error from previous steps — which is why VAR forecasts degrade quickly beyond 3-5 steps.
- **TimescaleDB adds real value over plain PostgreSQL.** Automatic partitioning, time-based queries, and continuous aggregates make it worth the setup cost for any time series workload.

---

## Key Insight

> "Forecasting one time series is statistics. Forecasting multiple correlated series is systems thinking — the relationships between series carry more information than any single series alone."

---

## Running

```bash
pip install -r requirements.txt
python forecast.py
```
