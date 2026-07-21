# Build Your First RAG System From Scratch

A minimal but complete Retrieval-Augmented Generation system with FAISS indexing, confidence scoring, source citations, and interactive Q&A — no LangChain, no frameworks.

---

## Problem Statement

RAG tutorials overwhelm with frameworks and abstractions. This project strips RAG down to its essentials: how do you chunk text, embed it, index it with FAISS, retrieve relevant passages, and generate answers — all in ~160 lines of Python with no framework dependencies?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Embeddings** | `all-MiniLM-L6-v2` via sentence-transformers | 384-dim, fast on CPU, strong semantic similarity scores |
| **Vector index** | FAISS IndexFlatL2 | Simple, exact search — no approximation needed for small knowledge bases |
| **QA model** | `distilbert-base-cased-distilled-squad` | Extractive QA: reads the context and extracts the answer span. More reliable than generative for factual questions |
| **Chunking** | Fixed-size (100 words, 20 overlap) | Simple sliding window; overlap prevents losing context at chunk boundaries |
| **Persistence** | Pickle (FAISS index + chunks) | One file saves the entire index — no database needed for small-scale use |
| **Confidence** | Composite: `qa_score × retrieval_confidence` | Combines how confident the QA model is with how well the passage matched the query |

---

## What I Learned

- **RAG is surprisingly simple at its core.** Embed → index → retrieve → generate. The complexity is in the details (chunking strategy, confidence thresholds, prompt design), not the architecture.
- **Composite confidence scoring catches bad answers.** When the QA model is confident (0.95) but the retrieval match is weak (0.31), multiplying them gives 0.29 — correctly flagging low-confidence answers.
- **The "I don't know" threshold is critical.** Without a minimum confidence cutoff (0.3), the system would confidently extract random text spans as answers. Knowing when *not* to answer is as important as answering correctly.
- **FAISS persistence is trivial.** Pickling the index + chunks list gives you instant reload on subsequent runs — no re-embedding needed.

---

## Key Insight

> "The most important feature of a RAG system isn't how it answers — it's how honestly it says 'I don't know.'"

---

## Running

```bash
pip install numpy faiss-cpu sentence-transformers transformers torch
python rag_from_scratch.py
```
