# Synthetic Data Generation with Generative AI

A PyTorch GAN that learns the distribution of app usage data (usage time, notifications, times opened) and generates realistic synthetic records — with real vs. synthetic comparison.

---

## Problem Statement

App usage data is sensitive — you can't share it without privacy concerns. But you need data to test analytics pipelines, train models, or share with partners. Can a GAN learn the joint distribution of Usage, Notifications, and Times Opened well enough to generate synthetic records that preserve the relationships between these features?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Architecture** | MLP GAN (Generator: 10→128→128→3, Discriminator: 3→128→64→1) | Simple enough to train on CPU in 400 epochs; the 3-output architecture matches the 3 numeric features |
| **Normalization** | MinMaxScaler to [0, 1] | Sigmoid output of generator matches this range; inverse transform restores original scale |
| **Training** | 400 epochs, batch size 32, LR 2e-4 | Enough for convergence on a 100-row dataset; more epochs would overfit |
| **Post-processing** | Clip + round to int | Real app usage data is discrete — raw GAN output must be cast to integers |
| **Evaluation** | Real vs. synthetic mean comparison | Quick sanity check: if means are wildly different, the GAN didn't learn the distribution |

---

## What I Learned

- **GANs on tiny datasets are unstable but instructive.** With only 100 rows, the discriminator can memorize the training data. The GAN still converges, but the generated data reflects memorization as much as generalization.
- **Batch normalization is essential for tabular GANs.** The generator's `BatchNorm1d` layers stabilized training significantly — without them, mode collapse happened within 50 epochs.
- **Post-processing is where the magic happens.** Raw GAN output: `Usage: 437.23, Notifications: 23.7, Times opened: 56.1`. After clipping and rounding: `Usage: 437, Notifications: 24, Times opened: 56`. The GAN learns the distribution; post-processing makes it realistic.
- **Real vs. synthetic mean comparison is a minimum viability check.** If `real_mean(Usage) ≈ 500` and `synthetic_mean(Usage) ≈ 480`, the GAN captured the scale. For production, you'd want KS tests and correlation matrices.

---

## Key Insight

> "A GAN doesn't memorize your data — it learns the rules that generated your data. The synthetic output isn't copies; it's new samples from the same underlying distribution."

---

## Running

```bash
pip install torch pandas numpy scikit-learn
python gan.py
```
