# Build a Real-Time AI Assistant Using RAG + LangChain

A streaming RAG assistant that searches the web via DuckDuckGo, retrieves context, and generates answers using a local Ollama LLM — with full LangChain LCEL pipeline and logging.

---

## Problem Statement

ChatGPT without internet is limited to its training data. How do you build a real-time assistant that searches the web for current information and generates grounded answers — all running locally with no API keys?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Search** | DuckDuckGoSearchRun via LangChain | Free, no API key, privacy-friendly — works immediately without setup |
| **LLM** | Ollama with `qwen3.5` | Local, free, fast. Qwen3.5 handles RAG prompts well and supports streaming |
| **Pipeline** | LangChain LCEL (RunnableSequence) | Composable: `search → prompt → llm → parser`. Each step is independently testable |
| **Streaming** | `chain.stream()` with flush | Token-by-token output — gives the user immediate feedback instead of waiting for the full response |
| **Prompt design** | System prompt + search results + question | Explicitly instructs the LLM to answer from search results, and mention when results are limited |
| **Logging** | Dual handler (file + console) | Every step is logged: search queries, result lengths, response times — essential for debugging |

---

## What I Learned

- **LCEL chains are more readable than LLMChain.** The pipe operator (`|`) makes the data flow explicit: `search | prompt | llm | parser`. Reading the chain is like reading the architecture diagram.
- **Streaming transforms the UX.** Token-by-token output makes a 10-second response feel instant. The user sees progress from the first token — critical for perceived performance.
- **Search quality limits the entire pipeline.** If DuckDuckGo returns irrelevant results, no LLM can fix it. The search step is the bottleneck — not the generation step.
- **Ollama's model loading is the cold-start cost.** First query takes 10-30 seconds (model load). Subsequent queries take 2-5 seconds. Pre-loading or keeping the model in memory is essential for interactive use.

---

## Key Insight

> "A real-time AI assistant isn't about the LLM — it's about the search. The LLM generates text; the search determines whether that text is current, relevant, and grounded in reality."

---

## Running

```bash
# Requires Ollama running locally
ollama pull qwen3.5
pip install langchain langchain_community langchain_core langchain_ollama duckduckgo-search
python real_time_assistant.py
```
