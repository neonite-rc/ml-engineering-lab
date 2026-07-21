# Building Synthetic Medical Records using GANs

Generating realistic synthetic patient records (age, BMI, glucose, blood pressure) using a vanilla GAN in PyTorch — without touching real patient data.

---

## Problem Statement

Medical data is locked behind privacy regulations (HIPAA, GDPR). You can't share it, you can't publish it, and you can't use it to train models without lengthy approval processes. Can a GAN learn the statistical distribution of patient records well enough to generate synthetic data that preserves correlations (e.g., BMI ↔ glucose) without memorizing real patients?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Architecture** | Vanilla GAN (MLP generator + discriminator) | Simple enough to train on CPU in 100 epochs, complex enough to capture basic distributions |
| **Preprocessing** | MinMaxScaler to [-1, 1] | GAN outputs use Tanh activation, so training data must match this range |
| **Loss** | BCELoss (binary cross-entropy) | Standard for GAN training — measures real vs. fake discrimination |
| **Noise dimension** | 10 | Small latent space forces the generator to learn compressed representations |
| **Feature encoding** | Gender as categorical code (0/1) | Simple label encoding; post-processing rounds to nearest integer and maps back to Male/Female |

---

## What I Learned

- **GANs on tabular data are underexplored but viable.** Most GAN tutorials focus on images. Training a GAN on 5-column tabular data is simpler but requires careful normalization and post-processing to maintain data types.
- **The generator learns correlations, not just distributions.** After 100 epochs, generated records showed realistic BMI-glucose relationships — the GAN didn't just sample from marginal distributions independently.
- **Post-processing is non-negotiable.** The raw GAN output produces continuous values for discrete fields (Gender). Rounding + mapping + range clipping is essential before the synthetic data is usable.
- **100 epochs is enough for demonstration, not production.** The discriminator loss converges but the generated data would need validation against real distributions (KS test, correlation matrices) for clinical use.

---

## Key Insight

> "Synthetic data isn't about fooling a discriminator — it's about preserving the statistical relationships that make data meaningful, while ensuring no single real record can be reconstructed."

---

## Running

```bash
pip install torch pandas numpy scikit-learn
python medical_gan.py
```
