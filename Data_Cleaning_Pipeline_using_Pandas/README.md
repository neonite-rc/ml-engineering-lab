# Data Cleaning Pipeline using Pandas

A structured, auditable data cleaning pipeline for tabular data — handling missing values, type optimization, string standardization, and outlier capping with a full operation log.

---

## Problem Statement

Every ML project starts with dirty data: missing values, inconsistent strings, wrong types, outliers. But ad-hoc cleaning scripts are unrepeatable and untraceable. How do you build a cleaning pipeline that's both automated and auditable — so you can tell exactly what changed and why?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Missing values** | Categorical: mode; Numeric: median | Mode preserves the most common category; median is robust to outliers (unlike mean) |
| **Type optimization** | Downcast int64→int16/int32, float64→float32 | Reduces memory by 50-75% with zero information loss for most datasets |
| **String standardization** | Strip + title case | Eliminates "Male" vs " male " vs "MALE" inconsistencies |
| **Outlier handling** | Percentile capping (99th) | Caps extreme values without removing rows — preserves sample size |
| **Audit trail** | Operation log (list of strings) | Every transformation is logged with details — enables debugging and reproducibility |

---

## What I Learned

- **Memory optimization is free performance.** Downcasting int64 to int16 (when values fit) reduced the loan prediction dataset memory by 60%. This matters when scaling to millions of rows.
- **The cleaning log is the most valuable artifact.** When a model produces weird results, the log tells you exactly what happened: "Loan_Amount_Term: 14 values filled with median 360." This is debugging gold.
- **String standardization is deceptively complex.** Title-casing "O'CONNOR" → "O'Connor" is correct, but "McDONALD" → "Mcdonald" loses information. For production systems, you'd want locale-aware casing.
- **Percentile capping is safer than IQR for small datasets.** With only 600 rows, IQR-based outlier removal could discard 10%+ of data. Capping at the 99th percentile preserves the distribution shape while limiting extreme values.

---

## Key Insight

> "Data cleaning isn't a one-time task — it's a documented, repeatable process. If you can't explain what you changed and why, you're not cleaning data, you're mutating it."

---

## Running

```bash
pip install pandas numpy
python data_cleaning_pipeline.py
```
