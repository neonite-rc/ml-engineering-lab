# MLOps Pipeline using Apache Airflow

An Airflow DAG that orchestrates a two-stage ML pipeline: data preprocessing followed by model training with automated evaluation metrics.

---

## Problem Statement

ML pipelines fail silently when run as standalone scripts. Data drifts, models decay, and nobody notices until a stakeholder complains. How do you build a scheduled, observable, retry-capable pipeline that automatically preprocesses data and trains a model on a daily cadence?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Orchestration** | Apache Airflow with PythonOperator | Industry standard for data pipelines; DAG definition, scheduling, retries, and UI out of the box |
| **Pipeline structure** | Linear: preprocess → train | Simple dependency ensures training always runs on freshly preprocessed data |
| **Model** | RandomForestRegressor (100 trees) | Robust baseline, handles missing values, no scaling needed — appropriate for a demo pipeline |
| **Evaluation** | MAE (Mean Absolute Error) | Interpretable metric: "off by X units on average" — easier to explain to stakeholders than RMSE |
| **Scheduling** | Daily (`timedelta(days=1)`) | Demonstrates Airflow's scheduling; real pipelines might run hourly for streaming data |

---

## What I Learned

- **Airflow is overkill for a two-step pipeline — and that's the point.** The value isn't the pipeline complexity; it's the infrastructure: retries, logging, scheduling, and the web UI. Adding a third step (deployment, notification, A/B testing) requires zero architectural changes.
- **`catchup=False` is essential for new DAGs.** Without it, Airflow tries to backfill every day since `start_date`, launching hundreds of historical runs. This is a common gotcha that can overwhelm a new Airflow instance.
- **Dummy data generation on first run is pragmatic.** The pipeline creates synthetic screentime data if none exists — this means the pipeline works immediately after setup without requiring a real data source.
- **The >> operator is the pipeline's contract.** `preprocess_data >> train_model` explicitly declares that training depends on preprocessing. Airflow handles the execution order, retries, and failure propagation.

---

## Key Insight

> "MLOps isn't about making ML harder — it's about making ML observable. A pipeline that logs its steps, retries on failure, and runs on schedule is worth 10x a model that trains perfectly but runs once."

---

## Running

```bash
# Requires Apache Airflow installed
airflow dags list
airflow dags test ml_pipeline_dag
```
