# Data Augmentation using LLMs

Generating synthetic training rows from a small seed dataset by learning data patterns and producing validated, schema-consistent outputs — no real LLM API required.

---

## Problem Statement

Small datasets kill ML models. Collecting more data is expensive. Can an LLM (or a simulated pattern-learning system) generate new training rows that preserve the statistical properties of the original data — with automatic validation to catch garbage outputs?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Augmentation strategy** | Pattern learning from structured prompts | Parses CSV-formatted examples, learns the salary/experience ratio, and generates new rows that follow the same linear trend |
| **Validation pipeline** | Schema check → type check → range check → sanity check → dedup | Each layer catches a different failure mode: missing columns, non-numeric values, impossible ranges, illogical correlations, duplicate IDs |
| **Output format** | DataFrame (pandas) | Structured output enables immediate statistical analysis and downstream ML use |
| **Simulated LLM** | Rule-based pattern learner | Demonstrates the augmentation workflow without API costs; production version would swap in GPT-2/LLaMA |

---

## What I Learned

- **Validation is more important than generation.** The augmentor generates plausible data, but the 5-step validation pipeline (schema → type → range → sanity → dedup) is what makes it trustworthy. Without validation, synthetic data introduces silent failures.
- **Pattern learning works for structured data.** From just 5 example rows, the system learned that salary ≈ years_of_experience × 10000 (±15% noise). This is surprisingly effective for tabular augmentation.
- **The sanity check is the most underrated step.** Range validation catches "salary = -5000" but not "salary = 200000 with 1 year experience." The constraint `salary >= experience × 8000` catches logically inconsistent rows.
- **Augmented data needs the same EDA as real data.** Generated rows can have correct individual values but wrong correlations. Always plot, always check distributions.

---

## Key Insight

> "Generating synthetic data is easy. Generating synthetic data you can trust is a validation engineering problem."

---

## Running

```bash
pip install pandas
python data_augmentation.py
```
