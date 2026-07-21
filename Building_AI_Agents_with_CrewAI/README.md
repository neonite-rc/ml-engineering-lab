# Building AI Agents with CrewAI

A multi-agent research-to-content pipeline using CrewAI (or a lightweight shim) with a local Mistral 7B model — demonstrating agent role specialization, task delegation, and sequential execution.

---

## Problem Statement

Single agents handle simple tasks, but complex workflows benefit from specialization: a researcher gathers information, a writer synthesizes it. CrewAI provides the framework for agent orchestration, but most examples rely on OpenAI APIs. How do you build a CrewAI pipeline that runs entirely locally?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **LLM** | Mistral 7B Instruct (GGUF Q4) via CTransformers | Quantized, runs on CPU in 4GB RAM — no API costs, full privacy |
| **Framework** | CrewAI with LiteCrew fallback | Real CrewAI when available; lightweight shim when dependencies conflict — the pipeline never fails |
| **Agent roles** | Researcher → Writer | Simplest useful specialization: gather → synthesize |
| **Context management** | Truncation to 1200 chars | Keeps prompts within Mistral's context window — prevents silent truncation |
| **Response cleaning** | Terminal punctuation detection | Truncates at the last complete sentence — prevents half-finished outputs |

---

## What I Learned

- **The LiteCrew shim pattern is pragmatic.** CrewAI has heavy dependencies (Pydantic V1 conflicts with Python 3.14). The shim reproduces the essential API (Agent, Task, Crew) without the dependency chain — the pipeline runs everywhere.
- **Agent specialization works even with simple prompts.** The Researcher gets "analyze trends"; the Writer gets "write a blog post." The same LLM produces different-quality output based on the role prompt alone.
- **Context truncation is a deployment concern, not a model concern.** Mistral's 2048-token context means prompts must be carefully sized. The `truncate_context()` function prevents the most common failure: silent context overflow.
- **Sequential execution is the right default.** The Researcher runs first, produces output, which becomes the Writer's context. Parallel agent execution adds complexity that's only justified when tasks are truly independent.

---

## Key Insight

> "Multi-agent systems are only as good as the handoff between agents. The researcher's output is the writer's input — if the handoff is sloppy, the entire pipeline degrades."

---

## Running

```bash
# Requires Mistral GGUF model (see MODEL_PATH)
pip install langchain_community
python crewai_agents.py
```
