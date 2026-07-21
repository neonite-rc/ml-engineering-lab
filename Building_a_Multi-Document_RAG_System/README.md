# Multi-Document RAG System

A single-file web server that ingests multiple PDFs, indexes them with TF-IDF, and answers questions via extractive retrieval with evidence tracing.

---

## Problem Statement

Single-document RAG is straightforward, but real-world use means querying across 5-10 documents simultaneously. How do you chunk heterogeneous PDFs, rank passages across documents, and return answers with source citations — all without a single ML model dependency?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Retrieval** | TF-IDF + cosine similarity | Zero model download, instant indexing, and competitive recall for resume/CV-style documents |
| **Chunking** | Sentence-window (3 sentences, 1 overlap) | Preserves sentence-level context while keeping chunks small enough for precise matching |
| **Answer extraction** | Rule-based + extractive | Regex patterns for emails, phones, names; sentence scoring for general queries. Avoids LLM dependency entirely |
| **Web interface** | Single-file HTTP server (stdlib) | No Flask, no FastAPI — just `http.server` with inline HTML/CSS/JS. One file, zero deps beyond numpy/sklearn |
| **Document limit** | 10 PDFs max | Keeps TF-IDF matrix manageable and query latency under 100ms |

---

## What I Learned

- **TF-IDF is underrated for structured documents.** For resumes and contracts where exact keyword matching matters (skills, certifications, dates), TF-IDF outperforms semantic search that might confuse "Python" (language) with "python" (snake).
- **Extractive answers beat generated ones for factual extraction.** When a user asks "What's the email?", regex extraction from the top chunk is more reliable than an LLM generating "The email is..." — especially without an LLM.
- **Intent inference improves ranking.** Mapping query terms to domain-specific expansions ("project" → "built, developed, implemented") significantly boosted relevance for resume queries.
- **The confidence threshold matters.** Two thresholds — `MIN_CONFIDENCE` (0.05) for general queries and `STRICT_MIN_CONFIDENCE` (0.12) for definitive answers — prevented the system from confidently returning garbage.

---

## Key Insight

> "Not every RAG system needs an LLM. For structured document querying, a well-tuned TF-IDF index with smart extraction rules can outperform a 7B-parameter model — and it runs in 50ms."

---

## Running

```bash
pip install numpy scikit-learn pypdf
python rag_server.py
# Open http://127.0.0.1:8000
```
