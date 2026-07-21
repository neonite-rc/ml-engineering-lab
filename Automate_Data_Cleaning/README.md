# Automate Data Cleaning

A Rust-based data cleaning pipeline using Polars for high-performance CSV processing — standardizing columns, removing duplicates, and trimming whitespace.

---

## Problem Statement

Python data cleaning with pandas is the default, but it's slow on large datasets and requires a Python environment. Can Rust + Polars deliver the same cleaning operations at 10-100x speed, with compiled binary deployment?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Language** | Rust | Compiled, memory-safe, no runtime dependencies — deploy as a single binary |
| **DataFrame library** | Polars | Rust-native, lazy evaluation, multi-threaded — 10-100x faster than pandas for I/O-bound cleaning |
| **Cleaning operations** | Standardize names → Drop duplicates → Trim whitespace | The three most common cleaning operations; covers 80% of real-world needs |
| **Column naming** | Lowercase + underscore + trim | Consistent convention: "Loan_ID" → "loan_id", "Recovery Amount" → "recovery_amount" |
| **Output** | CSV via Polars CsvWriter | Same format as input — the pipeline is a clean-in, clean-out transformation |

---

## What I Learned

- **Polars' lazy API is worth the learning curve.** `df.lazy().with_column(...).collect()` defers computation until `collect()`, enabling query optimization. For a 3-step pipeline, the speedup is negligible — but it scales.
- **Rust's error handling forces robustness.** Every Polars operation returns `Result<T, PolarsError>`. This catches CSV parse errors, column-not-found errors, and type mismatches at compile time — not at 3 AM when the pipeline runs.
- **Column name standardization is more important than it seems.** "Loan_ID" vs "loan_id" vs "loan id" — inconsistent naming breaks downstream joins and analyses. Normalizing once at the top of the pipeline prevents cascading issues.
- **Compiled data cleaning is deployable anywhere.** The Rust binary runs on any Linux machine without Python, pip, or virtual environments. This matters for deployment in restricted environments.

---

## Key Insight

> "Data cleaning in Rust isn't about speed — it's about reliability. When a cleaning pipeline runs as a compiled binary, it either works or it doesn't. No dependency conflicts, no version mismatches, no 'works on my machine.'"

---

## Running

```bash
# Requires Rust toolchain
cargo build --release
./target/release/data-cleaning
```
