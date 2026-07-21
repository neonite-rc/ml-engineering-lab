# Building an Agentic RAG Pipeline

A query-routing RAG system that decides whether to retrieve from documents or answer directly — no paid APIs required.

---

## Problem Statement

Most RAG pipelines blindly retrieve from a vector store for every query, even when the answer is general knowledge. This wastes compute and introduces irrelevant context. The question: can a lightweight router decide *when* to retrieve and when to answer directly?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Router** | Keyword-based classifier | Simple, deterministic, fast. A production system would use an LLM-as-judge or trained classifier, but keywords work surprisingly well for domain-specific queries |
| **Embeddings** | `all-MiniLM-L6-v2` via sentence-transformers | Best quality-to-size ratio for CPU-only retrieval; 384-dim embeddings, 80MB model |
| **Vector store** | ChromaDB with persistence | Zero-config, local, persistent — perfect for prototyping without external infrastructure |
| **LLM** | `google/flan-t5-base` (250M params) | Free, local, and capable of instruction following. Small enough for CPU inference |
| **Document processing** | PyPDF + RecursiveCharacterTextSplitter | LangChain's splitter handles edge cases (headers, tables) better than naive chunking |

---

## What I Learned

- **The router is the most underrated part of RAG.** A keyword router correctly sent "What is 2 + 2?" to direct generation and "What is the capital of France?" to RAG. For a production system, an LLM-as-judge or trained intent classifier would replace this.
- **flan-t5-base struggles with short context.** When the PDF chunk only contained one sentence about the French Revolution, the model output "(iii)." — garbage. Larger context or a larger model (flan-t5-large) would fix this.
- **LangChain's API is moving fast.** `langchain.text_splitter` moved to `langchain_text_splitters`, `RetrievalQA` chain was removed entirely, and `text2text-generation` pipeline was dropped from transformers. Reading changelogs is mandatory.
- **Offline-first RAG is totally viable.** Zero API keys, zero cloud dependency — the entire pipeline runs locally with open models.

---

## Key Insight

> "The intelligence of a RAG system isn't in the retrieval or the generation — it's in knowing which one to use."

---

## Running

```bash
pip install -r requirements.txt
python agentic_rag.py
```
