# Document Analysis using LLMs

A hardware-aware document analysis system with parent-child chunking, ChromaDB vector storage, and FLAN-T5 generation — automatically adapting its strategy based on GPU availability.

---

## Problem Statement

Document QA systems typically use one chunking strategy regardless of hardware. On GPU, you can afford sophisticated parent-child chunking for better context. On CPU, you need simpler, faster approaches. How do you build a system that automatically adapts its processing strategy to available hardware?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Chunking** | Parent-child (GPU) / Standard (CPU) | GPU enables deeper hierarchical chunking with parent context; CPU uses simpler fixed-size chunks for speed |
| **Vector store** | ChromaDB with persistent storage | Local, zero-config, supports metadata filtering for parent-child relationships |
| **Generator** | `google/flan-t5-large` with beam search | 4-beam search + length penalty produces more coherent answers than greedy decoding |
| **Hardware detection** | Auto-detect CUDA, configure chunk sizes | GPU: 1024 parent / 256 child. CPU: 512 fixed. Matches processing depth to available compute |
| **PDF extraction** | PyMuPDF (fitz) | Faster and more reliable than PyPDF2 for text extraction, especially with complex layouts |

---

## What I Learned

- **Parent-child chunking genuinely improves answer quality.** When the retriever finds a relevant child chunk, including its parent context gives the LLM 4x more surrounding text to work with — reducing "I don't have enough context" responses.
- **Hardware-aware design isn't optional — it's essential.** A system that requires GPU for all users excludes 80% of developers. The automatic fallback to standard chunking on CPU means the same code works everywhere.
- **Beam search is worth the compute.** For document QA, `num_beams=4` with `length_penalty=1.5` produces answers that read naturally, while greedy decoding often produces truncated or repetitive output.
- **ChromaDB telemetry logs are noisy in offline environments.** Disabling `anonymized_telemetry` in the PersistentClient settings eliminates retry logs when there's no network access.

---

## Key Insight

> "The best document analysis system is the one that works on the hardware you actually have — not the hardware you wish you had."

---

## Running

```bash
pip install -r requirements.txt
python document_analysis.py
```
