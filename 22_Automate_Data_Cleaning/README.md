# Automated Data Cleaning (Rust)

Deliberate language choice for performance-critical preprocessing.

---

## Problem Statement

Python data cleaning is slow on large datasets. 1M rows takes minutes. Need sub-second preprocessing for real-time pipelines.

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Language** | Rust over Python | 10x speedup for I/O-bound operations |
| **Parsing** | serde_json | Fast, safe JSON parsing |
| **Output** | Parquet over CSV | Columnar format, faster reads |

---

## What I Learned

- Rust is worth the learning curve for performance-critical code
- The borrow checker prevents entire classes of bugs
- Python interop (PyO3) makes Rust practical for data pipelines

---

## Key Insight

> "Choose the right tool for the job. Sometimes that means leaving Python."

---

## Running

```bash
cargo run --release -- input.json output.parquet
```
