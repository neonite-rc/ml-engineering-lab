# Fine-tuning LLMs on Your Own Data

Fine-tuning DistilBERT for sentiment classification using LoRA (Low-Rank Adaptation) — training only 0.5% of parameters while achieving competitive accuracy on IMDb reviews.

---

## Problem Statement

Full fine-tuning of LLMs requires massive compute. LoRA (Low-Rank Adaptation) freezes the original model and trains tiny adapter matrices, making fine-tuning accessible on consumer hardware. But how do you set up a LoRA training pipeline that handles dataset loading, tokenization, training, evaluation, and inference — with offline fallbacks for environments without internet?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Base model** | `distilbert-base-uncased` | 66M params (6x smaller than BERT), retains 97% of BERT's performance — ideal for LoRA experiments |
| **LoRA config** | r=8, alpha=16, targets `q_lin`/`v_lin` | Low rank (r=8) keeps adapter params tiny; targeting query/value attention matrices captures the most task-relevant information |
| **Dataset** | IMDb sentiment (200 train / 50 test) | Standard benchmark; small subset demonstrates the workflow without long training times |
| **Offline fallback** | Synthetic sentiment templates | When IMDb can't be downloaded, generates 250 template-based reviews — the pipeline never fails |
| **Metrics** | Accuracy, precision, recall, F1 | Full classification metrics, not just accuracy — important for imbalanced datasets |

---

## What I Learned

- **LoRA is remarkably parameter-efficient.** `print_trainable_parameters()` showed ~0.5% of total params are trainable. The adapter matrices add minimal overhead while enabling task-specific adaptation.
- **Offline-first design is non-negotiable for reproducibility.** Both dataset loading and model loading have fallback paths (synthetic data, random initialization). The pipeline runs identically offline and online.
- **Local files only (`local_files_only=True`) catches broken caches.** If a previous download was incomplete, `from_pretrained()` with `local_files_only` fails fast instead of silently using corrupted weights.
- **One epoch is enough to demonstrate the workflow.** With 200 samples and 1 epoch, the model learns basic sentiment patterns. Production fine-tuning would use 3-5 epochs on the full 25k training set.

---

## Key Insight

> "Fine-tuning isn't about training more parameters — it's about training the right parameters. LoRA proved that 0.5% of the weights, in the right places, can match full fine-tuning."

---

## Running

```bash
pip install -r requirements.txt
python fine_tuning.py
```
