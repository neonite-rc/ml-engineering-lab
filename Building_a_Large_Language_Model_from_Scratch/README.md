# Building a Large Language Model from Scratch

A minimal GPT-style transformer built from scratch in PyTorch — implementing character-level tokenization, multi-head self-attention, feed-forward blocks, and autoregressive text generation.

---

## Problem Statement

GPT-like models seem like black boxes. This project demystifies them by building every component from raw PyTorch: token embeddings, positional embeddings, self-attention heads, multi-head attention, feed-forward networks, layer normalization, and the full transformer block — all in ~170 lines.

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Tokenization** | Character-level | Simplest possible tokenizer — no BPE, no vocabulary file. Demonstrates the concept without complexity |
| **Architecture** | GPT-style decoder-only transformer | The industry standard for language models; unidirectional attention, next-token prediction |
| **Attention** | Multi-head self-attention (4 heads, 64-dim) | 4 heads capture different relationship patterns; 64-dim per head keeps total at 256-dim |
| **Position encoding** | Learned embeddings (not sinusoidal) | Simpler to implement; works well for short sequences (block_size=32) |
| **Training** | Cross-entropy loss on next-token prediction | Standard language modeling objective — predict the next character given all previous characters |
| **Generation** | Autoregressive sampling with softmax | Sample from the model's probability distribution, append to context, repeat |

---

## What I Learned

- **Self-attention is just matrix multiplication.** `wei = q @ k.T / √C; wei = softmax(wei); out = wei @ v` — three lines of code that capture the core of GPT. The masking (causal attention) is just filling future positions with -inf.
- **Multi-head attention isn't mysterious.** It's just running several small attention operations in parallel and concatenating the results. Each head learns different relationship patterns (syntactic, semantic, positional).
- **Layer normalization is essential for training stability.** Without `LayerNorm`, the loss oscillates wildly. With it, training converges smoothly — the normalization prevents gradient explosion in deep stacks.
- **Character-level models produce surprisingly coherent text.** After 1000 steps on a tiny corpus, the model generates recognizable English words and basic grammar. Scaling to word-level tokenization and larger corpora would produce genuinely useful text.

---

## Key Insight

> "A language model is just a function that predicts the next token given all previous tokens. Everything else — attention, layers, normalization — is engineering to make that prediction accurate at scale."

---

## Running

```bash
pip install torch
python llm_from_scratch.py
```
