# Text Classification Pipeline

A text classification pipeline in R that categorizes newsgroup articles into topics using Naive Bayes — with both quanteda (production) and base-R (fallback) implementations.

---

## Problem Statement

Text classification is a foundational NLP task, but most tutorials rely on Python. How do you build a complete text classification pipeline in R — from raw text to trained model to evaluation — with a fallback that works without any external NLP packages?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Algorithm** | Naive Bayes | Fast, interpretable, works well on small datasets — the baseline every text classifier should beat |
| **Primary backend** | quanteda + quanteda.textmodels | Production-grade tokenization, DFM creation, and NB model — R's standard NLP toolkit |
| **Fallback** | Base R implementation | Custom tokenization, manual vocabulary, hand-built DFM — works with zero package installs |
| **Preprocessing** | Remove punctuation, numbers, symbols, stopwords | Standard NLP pipeline; reduces vocabulary size by 60-70% |
| **Evaluation** | Confusion matrix + accuracy | Simple and interpretable — shows exactly which topics are confused with each other |

---

## What I Learned

- **Base R fallback is surprisingly competitive.** For a small dataset, the hand-built Naive Bayes (with Laplace smoothing) achieved comparable accuracy to quanteda's implementation. The difference only matters at scale.
- **quanteda's tokenization is faster but not always better.** `quanteda::tokens()` handles edge cases (contractions, hyphens) that `strsplit` misses. But for simple text, the difference is negligible.
- **Stopword removal is dataset-dependent.** Removing "will" and "said" (common in newsgroup posts) improved classification, but removing "not" (negation) would hurt sentiment tasks. Stopword lists should be curated per domain.
- **The 80/20 train/test split is a minimum.** With only 5 articles in the sample dataset, the evaluation is unreliable. Production evaluation needs 100+ articles per class with stratified splitting.

---

## Key Insight

> "The best NLP pipeline is the one that works without the internet. A base-R fallback that runs anywhere is more valuable than a production system that fails when pip can't reach PyPI."

---

## Running

```bash
# With quanteda (recommended)
Rscript -e 'install.packages(c("quanteda", "quanteda.textmodels"), repos="https://cran.r-project.org")'
Rscript text_classification.R

# Without packages (base R fallback)
Rscript text_classification.R
```
