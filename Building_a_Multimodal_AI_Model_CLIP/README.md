# Building a Multimodal AI Model (CLIP)

Matching images to text descriptions using OpenAI's CLIP and cosine similarity in a shared embedding space.

---

## Problem Statement

How do you teach a machine that a red square and "a photo of a red square" are the same thing? CLIP solves this by embedding images and text into the same vector space. This project explores zero-shot image-text matching — no fine-tuning, no labeled dataset — just a pre-trained model and a cosine similarity calculation.

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Model** | `openai/clip-vit-base-patch32` via HuggingFace | OpenAI's CLIP is the standard for image-text alignment; Vit-Base is lightweight enough for CPU |
| **Embedding extraction** | Full forward pass (`model(**inputs)`) | `get_image_features()` returned unprojected 768-dim vectors in this transformers version — the full pass gives the projected 512-dim shared embeddings directly |
| **Similarity metric** | Cosine similarity (L2-normalized dot product) | Dot product on normalized vectors equals cosine similarity — computationally clean |
| **Pipeline** | Batch encode all captions + image in one pass | Single forward pass for both modalities is more efficient than separate encodes |

---

## What I Learned

- **CLIP's shared space is remarkably robust.** A solid red square was correctly matched to "a bright red colored image" over 4 other captions, including visually unrelated ones.
- **The HuggingFace API has quirks.** `get_image_features()` returns `BaseModelOutputWithPooling` (768-dim, unprojected) instead of the 512-dim projected tensor. Using `model(**inputs).image_embeds` is the reliable path.
- **Projection heads matter.** The vision encoder outputs 768-dim but text encoder outputs 512-dim. Both get projected to 512-dim through CLIP's linear heads — this shared projection is what makes cross-modal comparison possible.
- **Zero-shot classification is surprisingly practical.** No training data, no fine-tuning — just embeddings and a dot product.

---

## Key Insight

> "Multimodal understanding isn't about building a model that sees and reads — it's about finding the right shared representation where seeing and reading become the same operation."

---

## Running

```bash
pip install torch Pillow transformers
python clip.py
```
