# Build an AI Agent to Automate Your Research

A research agent that searches the web, scrapes content, embeds passages with sentence-transformers, ranks by cosine similarity, and generates abstractive summaries using a local Mistral 7B model.

---

## Problem Statement

Research involves reading dozens of pages to answer one question. Can an AI agent automate this pipeline: search → scrape → chunk → embed → rank → summarize — using only local models with no API costs?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Web scraping** | BeautifulSoup + requests | Lightweight, no headless browser needed — extracts clean text from Wikipedia and docs |
| **Embeddings** | `all-MiniLM-L6-v2` via sentence-transformers | Fast semantic ranking; 384-dim, runs on CPU in milliseconds |
| **Ranking** | Cosine similarity (top-3 retrieval) | Simple but effective — the embedding model handles semantic similarity well |
| **Summarizer** | Mistral 7B Instruct (GGUF Q4) via ctransformers | Quantized to 4-bit, runs on CPU with 4GB RAM — local, private, no API |
| **Chunking** | 1000 chars, 200 overlap | Large enough for context, small enough for retrieval precision |
| **Fallback** | Concatenation-based summary | When Mistral isn't available, joins top passages — degrades gracefully |

---

## What I Learned

- **The research pipeline is the product, not the individual steps.** Each step (search, scrape, chunk, embed, rank, summarize) is simple alone. The value is in connecting them reliably with error handling at every boundary.
- **Quantized GGUF models change the game for local AI.** Mistral 7B at Q4_K_M fits in 4GB RAM and generates coherent summaries in 10-30 seconds on CPU. This wasn't possible two years ago.
- **Context window management is critical.** The prompt (question + context + template) can exceed Mistral's 2048-token context. Truncating passages to `MAX_PASSAGE_CHARS=500` and trimming the prompt to fit prevents silent failures.
- **Mock search is a valid development pattern.** `search_web()` returns hardcoded URLs for reproducible results. Replacing it with DuckDuckGo/SerpAPI is one function swap — the rest of the pipeline is identical.

---

## Key Insight

> "The most powerful AI agent isn't the one with the most tools — it's the one that can chain simple steps reliably, with graceful degradation at every failure point."

---

## Running

```bash
pip install -r requirements.txt
# Requires Mistral GGUF model (see MODEL_PATH env var)
python research_agent.py
```
