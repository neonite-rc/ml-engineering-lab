# Multi-Document RAG System

Retrieval-Augmented Generation across multiple documents with intelligent routing.

---

## Problem Statement

Single-document RAG works, but real-world queries span multiple documents. How do you retrieve relevant chunks from 10+ documents and generate coherent answers?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Retrieval** | Hybrid search (BM25 + semantic) | Best of both worlds: keyword precision + semantic understanding |
| **Chunking** | 512 tokens, 50 overlap | Tested 256/512/1024 — 512 balanced recall and precision |
| **Ranking** | Cross-encoder re-ranker | Initial retrieval is fast, re-ranking is accurate |

---

## What I Learned

- Chunking strategy affects retrieval quality more than embedding model
- Metadata filtering (document type, date) improves precision by 20%
- Cross-encoder re-ranking adds 50ms but improves answer quality significantly

---

## Key Insight

> "The hardest part of multi-document RAG isn't retrieval — it's knowing which document to retrieve from."

---

## Running

```bash
pip install -r requirements.txt
python rag_server.py
```
