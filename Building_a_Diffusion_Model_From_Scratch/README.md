# Building a Diffusion Model From Scratch

A from-scratch implementation of DDPM (Denoising Diffusion Probabilistic Model) on MNIST — with a custom U-Net, forward noising visualization, reverse denoising, and nearest-neighbor quality evaluation.

---

## Problem Statement

Diffusion models power DALL-E and Stable Diffusion, but most implementations hide the core mechanics behind libraries. This project builds every component from scratch: the noise schedule, forward diffusion, U-Net architecture, training loop, and reverse sampling — to understand exactly how images emerge from noise.

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Diffusion framework** | DDPM (Ho et al., 2020) | The foundational paper — simple, well-understood, and the basis for all modern diffusion models |
| **Noise schedule** | Linear beta (1e-4 → 0.02) over T=200 steps | Standard schedule from the paper; 200 steps is enough for MNIST (simpler than CIFAR/ImageNet) |
| **Architecture** | Simplified U-Net (no timestep embedding) | Encoder-decoder with skip connections — captures spatial structure; timestep conditioning omitted for clarity |
| **Training** | MSE loss between true noise and predicted noise | The model learns to predict what noise was added — then subtracts it during generation |
| **Evaluation** | Nearest-neighbor distance to MNIST | Quantitative check: generated samples should be closer to real digits than random noise |
| **Visualization** | Forward noising + reverse denoising step images | Visual proof that the process works: clean → noisy → clean |

---

## What I Learned

- **The forward process is trivial; the reverse process is the art.** Adding noise is just `x_t = √ᾱ_t · x_0 + √(1-ᾱ_t) · ε`. Learning to reverse it requires the U-Net to understand image structure at every noise level.
- **U-Net skip connections are essential.** Without them, the bottleneck loses spatial information. The `torch.cat([x, s2], dim=1)` skip connections preserve fine-grained details through the encoder-decoder path.
- **T=200 is enough for MNIST.** The paper uses T=1000, but MNIST's simplicity (binary-ish, low-resolution) means convergence happens faster. T=200 trains in ~200 steps on CPU.
- **Nearest-neighbor evaluation catches mode collapse.** If all generated digits are "1", the NN distances to real "1"s would be small but the label histogram would be skewed. The histogram provides a quick quality check without a human evaluator.

---

## Key Insight

> "Diffusion models don't generate images — they learn to remove noise. The generation process is just iteratively denoising random static until it looks like data the model has seen before."

---

## Running

```bash
pip install torch torchvision
python diffusion_from_scratch.py
# Output in artifacts/ and generated_digit.png
```
