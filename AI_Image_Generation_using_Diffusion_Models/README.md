# AI Image Generation using Diffusion Models

A CLI tool for generating images from text prompts using Stable Diffusion, with GPU detection, model selection, and configurable generation parameters.

---

## Problem Statement

Diffusion models have democratized image generation, but most tutorials hide the engineering behind a single `pipeline()` call. This project builds a production-style CLI that handles device detection, model loading, parameter validation, and generation — exposing the real workflow of deploying diffusion models.

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Pipeline** | HuggingFace `DiffusionPipeline` | Standard interface for all Stable Diffusion models — swap model IDs to change style |
| **Default model** | `runwayml/stable-diffusion-v1-5` | Best quality-to-VRAM ratio; widely supported, well-documented |
| **Device detection** | Auto-detect CUDA, fallback to CPU | GPU generation: ~10s. CPU generation: ~5min. Knowing which you're on before loading the model matters |
| **Parameter validation** | Pre-generation checks | Catches invalid guidance scales (1-20), step counts (1-150), and empty prompts before wasting GPU time |
| **Architecture** | Engine (`engine.py`) + CLI (`diffusion_image_gen.py`) | Separates generation logic from interface — engine can be used programmatically |

---

## What I Learned

- **Model loading is the bottleneck, not generation.** Loading Stable Diffusion v1.5 takes 30-60 seconds. Once loaded, each generation is 5-15 seconds on GPU. Pre-loading and keeping the pipeline in memory is essential for interactive use.
- **Negative prompts are surprisingly powerful.** Adding `--negative "blurry, low quality, watermark"` improved output quality more than increasing inference steps from 50 to 100.
- **Guidance scale is the quality knob.** Scale 7.5 is a good default, but pushing to 10-12 produces sharper images at the cost of diversity. Below 5, images become blurry and generic.
- **CPU generation is viable for testing.** At 150 steps on CPU, generation takes ~5 minutes — slow but functional for parameter tuning and prompt iteration without a GPU.

---

## Key Insight

> "The difference between a demo and a product in diffusion models isn't the model — it's handling the 30-second model load, the GPU memory check, and the parameter validation that prevents wasting a minute of compute on an invalid prompt."

---

## Running

```bash
pip install -r requirements.txt
python diffusion_image_gen.py "a cat wearing a wizard hat"
python diffusion_image_gen.py --list-models
```
