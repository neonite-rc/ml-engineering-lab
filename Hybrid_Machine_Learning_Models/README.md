# Hybrid Machine Learning Models

Combining PyTorch LSTM with Linear Regression in an ensemble for stock price prediction — leveraging temporal pattern learning and simple lag-based forecasting.

---

## Problem Statement

LSTMs capture complex temporal patterns but can overfit on small datasets. Linear regression with lag features is stable but misses non-linear relationships. Can combining both models in a simple average ensemble produce better predictions than either alone?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Deep model** | LSTM (1 layer, 32 hidden units) | Small architecture prevents overfitting on 200 data points; captures sequential dependencies |
| **Linear model** | LinearRegression with 3 lag features | Lag-1/2/3 features capture short-term momentum — stable baseline that doesn't need GPU |
| **Ensemble** | Simple average of LSTM + LR predictions | No learned weights — avoids overfitting the ensemble itself. Works surprisingly well when models are uncorrelated |
| **Scaling** | MinMaxScaler to [0, 1] | Required for LSTM convergence; inverse-transformed for interpretable output |
| **Window size** | Dynamic: min(30, len(data)//2) | Adapts to dataset size — prevents LSTM from having more parameters than training samples |

---

## What I Learned

- **Small LSTMs outperform large ones on small data.** 32 hidden units with 1 layer trained for 40 epochs on 200 points. Going bigger (128 units, 3 layers) caused overfitting — the loss dropped to near-zero on training but predictions were worse.
- **Lag features are a surprisingly strong baseline.** Linear regression with 3 lag features captured most of the short-term signal. The LSTM added value mainly at turning points where lag features lag by definition.
- **Simple ensembling is underrated.** No stacking, no meta-learner — just `(lstm + lr) / 2`. When one model fails, the other often compensates. The ensemble's RMSE was consistently lower than either individual model.
- **Dynamic window sizing prevents degenerate cases.** With 200 data points, a 30-step window leaves only 169 training samples. The `min(30, len(data)//2)` heuristic ensures at least half the data is available for training.

---

## Key Insight

> "The best model isn't the most complex one — it's the one that fails where the other succeeds. Hybrid models work because different architectures have different blind spots."

---

## Running

```bash
pip install torch pandas numpy scikit-learn
python hybrid_model.py
```
