# Topic Modelling using Python

Discovering hidden topics in a corpus of articles using Latent Dirichlet Allocation (LDA) — from text preprocessing to topic assignment to document labeling.

---

## Problem Statement

You have 100 articles and no labels. What topics do they cover? Topic modelling discovers latent thematic structure in unlabeled text — grouping words into topics and assigning each document a topic distribution. This project implements the full pipeline from raw text to interpretable topic labels.

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Algorithm** | LDA (Latent Dirichlet Allocation) | The gold standard for unsupervised topic discovery; probabilistic, interpretable, well-understood |
| **Vectorizer** | CountVectorizer (bag-of-words) | LDA requires count data (not TF-IDF) because it models word occurrence probabilities |
| **Preprocessing** | Lowercase → remove non-alpha → remove stopwords → min word length 3 | Reduces vocabulary noise; removes "the", "and", and ultra-short tokens |
| **Number of topics** | 5 (configurable) | Chosen to demonstrate topic separation; in production, use coherence scores to find optimal K |
| **Topic assignment** | Argmax of topic distribution | Each document gets its most probable topic — simple hard assignment |

---

## What I Learned

- **LDA's input must be word counts, not TF-IDF.** LDA models the generative process of word occurrences. TF-IDF downweights common words, which breaks LDA's probability model. This is a common mistake.
- **Stopword removal has outsized impact.** Without it, every topic's top words include "the", "is", "and" — making topics indistinguishable. Custom stopword lists (beyond sklearn defaults) improve topic quality.
- **Topic coherence matters more than topic count.** Having 5 topics with clear, distinct word distributions is better than 10 topics with overlapping, noisy distributions. Use `coherence_score` to evaluate.
- **Online learning mode (`learning_method='online'`) scales.** For large corpora, `online` updates the model incrementally instead of recomputing on the full matrix — essential for 100k+ documents.

---

## Key Insight

> "Topic modelling doesn't tell you what topics exist — it tells you what pattern of word co-occurrence best explains the variation across documents. The human interprets the pattern as a 'topic.'"

---

## Running

```bash
pip install pandas numpy scikit-learn
python topic_modelling.py
```
