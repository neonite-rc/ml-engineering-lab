# Diffusion Model from Scratch

U-Net + noise scheduler, converged to generate coherent images.

---

## Problem Statement

I could use Stable Diffusion, but I couldn't explain how the noise scheduling works, why U-Net is the architecture of choice, or how DDIM differs from DDPM.

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Architecture** | U-Net over Transformer | Spatial locality matters for images |
| **Scheduler** | DDIM over DDPM | 10x faster inference, comparable quality |
| **Loss** | MSE on noise prediction | Standard, well-understood |

---

## What I Learned

- DDIM converges faster but needs more steps for quality
- Noise scheduling matters more than model size
- Training stability requires careful learning rate scheduling

---

## Key Insight

> "Diffusion models are just denoising autoencoders with extra steps."

---

## Running

```bash
pip install -r requirements.txt
python diffusion_from_scratch.py
```
