# Build a Multi-Agent System With LangGraph

A state-graph multi-agent pipeline where a Researcher agent gathers information and a Writer agent synthesizes it into a blog post — using LangGraph and a local Ollama LLM.

---

## Problem Statement

Single-agent systems handle simple tasks, but complex workflows require multiple specialized agents that share state and coordinate. How do you build a multi-agent pipeline where agents pass information through a shared state graph — and how do you make it work with a local LLM?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Framework** | LangGraph (StateGraph) | Explicit state management between agents — you see exactly what data flows where |
| **LLM** | Ollama with `qwen3.5` | Local, free, no API keys. Qwen3.5 handles research and writing tasks competently |
| **State** | TypedDict (`topic`, `research_notes`, `final_output`) | Typed, explicit state — no hidden variables or implicit passing |
| **Graph structure** | Linear: researcher → writer → END | Simple but extensible — adding a "reviewer" node between writer and END requires one line |
| **Prompt design** | Task-specific templates | Researcher gets "Give me a brief overview"; Writer gets "Write a blog post based on these notes" |

---

## What I Learned

- **LangGraph's explicit state is its superpower.** Unlike LangChain's chain-of-chains, you see exactly what data each agent receives and produces. The `AgentState` TypedDict is the contract between agents.
- **Ollama availability is the first check.** The pipeline gracefully handles Ollama not running — it prints an error and exits cleanly instead of crashing. This is essential for demo code that might run in different environments.
- **Linear graphs are the right starting point.** Researcher → Writer → END is the simplest useful multi-agent system. Adding conditional edges (e.g., "if research is insufficient, loop back") adds complexity that should be justified by the use case.
- **LLM-as-agent works surprisingly well.** The Researcher agent doesn't have tools or memory — it just receives a topic and generates research notes. Yet the output is coherent and structured. Specialized prompts compensate for lack of infrastructure.

---

## Key Insight

> "Multi-agent systems aren't about having more agents — it's about having clear state contracts between them. A 2-agent system with explicit state is better than a 5-agent system with hidden dependencies."

---

## Running

```bash
# Requires Ollama running locally
ollama pull qwen3.5
pip install langchain langchain_community langchain_core langgraph langchain_ollama
python langgraph_system.py
```
