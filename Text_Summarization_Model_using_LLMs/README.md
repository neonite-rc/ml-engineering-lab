# Text Summarization Model using LLMs

Abstractive text summarization using T5 — converting long passages into concise summaries with beam search decoding and length penalty tuning.

---

## Problem Statement

Extractive summarization (picking important sentences) loses nuance. Abstractive summarization (generating new text) requires understanding. This project uses T5, a text-to-text transformer, to generate summaries that paraphrase and condense — not just extract.

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Model** | `t5-small` (60M params) | Lightweight enough for CPU, capable enough for coherent summaries. T5-base (220M) for better quality |
| **Task framing** | "summarize: " prefix | T5 was pre-trained on multiple text-to-text tasks; the prefix activates its summarization behavior |
| **Decoding** | Beam search (4 beams) | Explores 4 candidate summaries simultaneously — produces more coherent output than greedy decoding |
| **Length control** | `length_penalty=2.0`, `min_length=40`, `max_length=150` | Prevents both truncation and repetition; penalty encourages longer, more complete summaries |
| **Repetition prevention** | `no_repeat_ngram_size=2` | Blocks 2-gram repetition — the most common failure mode in abstractive summarization |

---

## What I Learned

- **T5's text-to-text framing is elegant.** "summarize: [text]" → "[summary]" — the same model handles classification, translation, and summarization just by changing the prefix. This framing was a breakthrough in NLP architecture.
- **Beam search quality depends heavily on length penalty.** Penalty 1.0 produces truncated summaries. Penalty 3.0 produces wordy ones. 2.0 hit the sweet spot for the JWST passage — concise but complete.
- **no_repeat_ngram_size is essential.** Without it, T5-small would produce "The telescope is the telescope is the telescope..." — a classic repetition loop. Setting size=2 blocks this while allowing natural word repetition.
- **Small models need careful prompting.** T5-small sometimes generates off-topic sentences when the input is long. Truncating to 512 tokens and using a clear prefix keeps it focused.

---

## Key Insight

> "Abstractive summarization isn't about compressing text — it's about understanding it well enough to say the same thing in fewer words. The model must comprehend before it can condense."

---

## Running

```bash
pip install transformers sentencepiece
python text_summarization.py
```
