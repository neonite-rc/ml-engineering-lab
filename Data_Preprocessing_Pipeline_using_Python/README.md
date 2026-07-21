# Data Preprocessing Pipeline using Python

A config-driven, sklearn-based preprocessing pipeline with custom outlier handling, column transformations, and automated quality reporting.

---

## Problem Statement

Data preprocessing scripts are typically hardcoded for one dataset. Change the columns, and the script breaks. This project builds a reusable pipeline where imputation strategies, outlier methods, and scaling approaches are all defined in a JSON config — making the same pipeline work across different datasets without code changes.

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Pipeline framework** | sklearn `Pipeline` + `ColumnTransformer` | Industry standard; serializable, composable, and works with `fit`/`transform` semantics |
| **Outlier handling** | Custom `OutlierHandler` transformer (IQR-based) | Replaces outliers with mean or caps at bounds — configurable per use case |
| **Configuration** | External `config.json` | Separates pipeline logic from dataset-specific decisions; one config file per dataset |
| **Quality reporting** | Before/after comparison (shape, missing values, statistics) | Quantifies exactly what the pipeline changed — essential for debugging and compliance |
| **CLI interface** | `argparse` with input/output/config/report flags | Production-ready command line: `python script.py -i data.csv -c config.json -v` |

---

## What I Learned

- **Custom sklearn transformers unlock real flexibility.** The `OutlierHandler` (inheriting `BaseEstimator` + `TransformerMixin`) slots into a `Pipeline` like any built-in transformer — this is the power of sklearn's protocol-based design.
- **Config-driven pipelines scale to team workflows.** When a teammate brings a new dataset, they write a JSON config instead of modifying Python code. This separation of concerns is worth the initial complexity.
- **Quality reports are non-negotiable.** The before/after comparison (missing values: 12→0, shape: 6×4→6×4) gives confidence that the pipeline did what it claimed. Without it, you're flying blind.
- **pandas 3.0 compatibility matters.** The `select_dtypes(include=['object', 'category', 'string'])` pattern is necessary because pandas 3.0 introduced the `StringDtype` — older code that only checks `'object'` silently misses string columns.

---

## Key Insight

> "The best preprocessing pipeline is the one you never have to rewrite. Invest in configuration and composability once, and every future dataset becomes a config file away."

---

## Running

```bash
pip install pandas numpy scikit-learn
python data_preprocessing_pipeline.py -i data.csv -c config.json -v
```
