# Enhanced RAG Pipeline for LLMs

A production-style RAG pipeline combining hybrid search (BM25 + FAISS), cross-encoder reranking, and query transformation for accurate, context-grounded answers.

---

## Problem Statement

Basic vector similarity search misses exact keyword matches, and pure keyword search misses semantic relationships. How do you combine both retrieval paradigms, rerank for precision, and transform queries for better coverage — all in one pipeline?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Hybrid search** | BM25 + FAISS dense (alpha=0.5) | BM25 catches exact terms ("Apollo 11", "Neil Armstrong"); FAISS catches semantic similarity. Equal weighting balances both |
| **Reranking** | `cross-encoder/ms-marco-MiniLM-L-6-v2` | Cross-encoders see query+document jointly — more accurate than bi-encoder similarity, worth the 50ms cost |
| **Chunking** | Sentence-aware with overlap (50 words, 10 overlap) | Splitting on sentence boundaries preserves context better than fixed-width chunking |
| **Query transformation** | Synonym expansion + question word removal | "What is the capital of France?" → also searches "capital of France" — catches documents that don't contain "what is" |
| **QA model** | `distilbert-base-cased-distilled-squad` | Extractive QA is more reliable than generative for factual answers with a known context |

---

## What I Learned

- **Hybrid search consistently outperforms either method alone.** BM25 nailed "Apollo 11" (exact match) while dense retrieval caught "moon landing" paraphrases. The alpha parameter is the key tuning knob.
- **Cross-encoder reranking is the highest-ROI improvement.** Adding reranking after hybrid search bumped average confidence from ~75% to 87.69%. The cross-encoder catches cases where BM25 and dense retrieval disagree on ranking.
- **Query transformation has diminishing returns.** Removing question words ("What is" → just the noun phrase) helped, but synonym expansion only helped for queries with known synonyms. A learned query expansion model would be better.
- **Sentence-aware chunking beats fixed-width.** Splitting at sentence boundaries kept facts intact — "Neil Armstrong was the first person to walk on the moon" stayed in one chunk instead of being split mid-sentence.

---

## Key Insight

> "The best RAG pipeline isn't the one with the most sophisticated components — it's the one where each component compensates for the others' blind spots."

---

## Running

```bash
pip install -r requirements.txt
python rag_pipeline.py
```
