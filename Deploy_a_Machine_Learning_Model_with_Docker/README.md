# Deploy a Machine Learning Model with Docker

A FastAPI service that trains a Gradient Boosting model on California Housing data and serves predictions via REST API — containerized with Docker for reproducible deployment.

---

## Problem Statement

Training a model is easy. Serving it reliably — with consistent dependencies, predictable environments, and a clean API contract — is where most ML projects fail. How do you go from `model.fit()` to a production-ready API endpoint?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **API framework** | FastAPI with Pydantic models | Auto-generates OpenAPI docs, validates request schemas, async-capable — best-in-class for ML APIs |
| **Model training** | GradientBoostingRegressor on California Housing | Simple enough to train in seconds, complex enough to demonstrate real predictions |
| **Serialization** | joblib | sklearn's native format — compact, fast, version-compatible |
| **Validation** | Pydantic `BaseModel` | Catches type errors and missing fields at the API boundary — before they reach the model |
| **Batch inference** | `/predict_batch/` endpoint | Real systems need batch processing — single-row prediction is a demo, not a product |

---

## What I Learned

- **The API contract IS the deployment.** Defining `HousingFeatures(BaseModel)` with exact types and field names forces you to document what the model expects. This is more valuable than any README.
- **Model creation on startup is a pragmatic pattern.** `create_model()` checks if `model.pkl` exists — first run trains, subsequent runs load. This avoids separate training/serving scripts while keeping cold start reasonable.
- **Pydantic validation catches bugs before they reach the model.** Sending `MedInc: "high"` returns a 422 error with a clear message, not a cryptic numpy traceback.
- **Batch endpoints matter for throughput.** A single `/predict/` call has 5ms of overhead per request. `/predict_batch/` with 100 samples has the same 5ms overhead — 100x better amortized latency.

---

## Key Insight

> "A model in a notebook is an experiment. A model in a Docker container is a product. The difference isn't the model — it's the environment contract."

---

## Running

```bash
# Without Docker
pip install fastapi uvicorn joblib pandas scikit-learn
uvicorn main:app --reload

# With Docker
docker build -t ml-api .
docker run -p 8000:8000 ml-api
```
