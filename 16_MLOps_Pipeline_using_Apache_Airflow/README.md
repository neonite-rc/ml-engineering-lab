# MLOps with Airflow

Full DAG with model registry, not just a single script.

---

## Problem Statement

Training models in notebooks is fine for experiments. But production ML needs orchestration, versioning, and monitoring. How do you productionize ML pipelines?

---

## Architecture

```
Airflow DAG → Data Ingestion → Feature Engineering → Training → MLflow Registry → Deployment
```

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Orchestration** | Airflow over cron | Visual DAGs, retry logic, monitoring |
| **Tracking** | MLflow | Model versioning, experiment comparison |
| **Container** | Docker | Reproducibility across environments |

---

## What I Learned

- DAG design is the hard part, not the ML code
- Model registry prevents "which model is in production?" confusion
- Retry logic saves hours of manual intervention

---

## Key Insight

> "The model is the easy part. The pipeline is the hard part."

---

## Running

```bash
docker-compose up -d
# Access Airflow UI at http://localhost:8080
```
