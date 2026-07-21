# RAG From Scratch

Custom retrieval engine without LangChain — proves I understand vector search.

---

## Problem Statement

LangChain abstracts too much. I needed to understand how vector search, chunking, and generation actually work together.

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Vector Store** | FAISS over ChromaDB | Faster for local experiments |
| **Embeddings** | sentence-transformers | Good balance of speed and accuracy |
| **Generation** | OpenAI API | Reliable, well-documented |

---

## What I Learned

- Building from scratch teaches you what LangChain hides
- The embedding step is the most important part
- Chunk overlap matters more than chunk size

---

## Key Insight

> "If you can't build it from scratch, you don't understand it."

---

## Running

```bash
pip install -r requirements.txt
python rag_from_scratch.py
```
