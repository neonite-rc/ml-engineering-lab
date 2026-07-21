# ML Engineering Lab

30 independent ML experiments spanning generative AI, NLP, computer vision, MLOps, and agentic systems. Built from scratch during BCA (2023–2026) to understand how these systems work under the hood before applying them at scale.

> **Next step:** Production systems in [ml-systems](https://github.com/neonite-rc/ml-systems).

---

## Problem Statement

I didn't understand how diffusion models actually work. I could use them, but I couldn't explain the noise scheduling, the U-Net architecture, or why DDIM converges faster than DDPM. This repo exists because I learn by building, not by reading.

---

## Experiment Table

| # | Topic | Hypothesis | Result | Key Learning |
|---|-------|-----------|--------|-------------|
| 01 | Multi-Document RAG | Chunking strategy affects retrieval quality | True | 512 tokens with 50 overlap works best |
| 02 | CLIP Alignment | Text-image alignment needs contrastive loss | True | Negative samples matter more than positive |
| 03 | Agentic RAG | Router improves retrieval relevance | True | Metadata filtering beats semantic search |
| 04 | RAG Pipeline | Context window size affects answer quality | Partially | Diminishing returns beyond 4K tokens |
| 05 | Synthetic Medical GANs | GANs can generate realistic medical records | True | Mode collapse is the real problem |
| 06 | RAG From Scratch | Custom retrieval beats LangChain default | True | Understanding the code matters |
| 07 | Data Augmentation | LLMs can augment training data effectively | True | Quality filtering is critical |
| 08 | Data Cleaning | Pandas pipeline beats manual cleaning | True | Reproducibility is the real benefit |
| 09 | Data Preprocessing | Python preprocessing is slow | True | Rust gives 10x speedup |
| 10 | Docker ML | Containerization improves reproducibility | True | Multi-stage builds save 50% size |
| 11 | Diffusion Images | DDPM can generate coherent images | True | 1000 steps is overkill, 500 suffices |
| 12 | Document Analysis | LLMs can extract structured info from docs | True | Prompt engineering matters most |
| 13 | Fine-tuning | LoRA is efficient for domain adaptation | True | Rank 8 outperforms rank 4 |
| 14 | Geospatial Clustering | DBSCAN works for location data | True | Epsilon parameter is dataset-dependent |
| 15 | Hybrid ML | Ensemble beats single model | True | Diversity matters more than size |
| 16 | MLOps Airflow | Airflow simplifies pipeline management | True | DAG design is the hard part |
| 17 | Time Series | Multivariate forecasting beats univariate | True | Feature selection is critical |
| 18 | News Collection | Web scraping is fragile | True | Respect robots.txt, add delays |
| 19 | Synthetic Data | GANs can generate tabular data | True | Privacy implications are real |
| 20 | Text Classification | Simple models often beat complex ones | True | TF-IDF + SVM is surprisingly good |
| 21 | Summarization | LLMs summarize better than extractive methods | True | But they hallucinate |
| 22 | Automated Cleaning | Rust preprocessing is worth the effort | True | 10x speedup for large datasets |
| 23 | Topic Modeling | BERTopic beats LDA | True | But needs more compute |
| 24 | Multi-Agent | Agents can collaborate effectively | True | Coordination is the hard part |
| 25 | Research Agent | Agents can automate research | Partially | Hallucination is a real problem |
| 26 | Game AI | DQN can learn to play games | True | Reward shaping is critical |
| 27 | Real-Time AI | RAG can work in real-time | True | Latency is the bottleneck |
| 28 | Diffusion From Scratch | U-Net + noise scheduler works | True | Convergence takes patience |
| 29 | CrewAI Agents | Multi-agent frameworks simplify development | True | But abstraction leaks |
| 30 | LLM From Scratch | Transformer decoder can be trained | True | Data quality matters more than size |

---

## Projects Worth Reading

| # | Project | Why It Stands Out |
|---|---------|-------------------|
| 06 | RAG From Scratch | Custom retrieval engine without LangChain — proves I understand vector search |
| 16 | MLOps with Airflow | Full DAG with model registry, not just a single script |
| 22 | Automated Data Cleaning (Rust) | Deliberate language choice for performance-critical preprocessing |
| 26 | LLM From Scratch | Transformer decoder trained on custom corpus, not Shakespeare |
| 28 | Diffusion Model from Scratch | U-Net + noise scheduler, converged to generate coherent images |

---

## What I Learned

### What Worked
- **Custom implementations** — Building RAG from scratch taught me more than using LangChain
- **Deliberate language choices** — Rust for preprocessing, Python for experimentation
- **MLOps early** — Airflow pipelines saved hours of manual runs
- **Evaluation metrics** — Tracking AUC-ROC, not just accuracy

### What Didn't Work
- **GANs for tabular data** — Mode collapse is real, need careful training
- **Complex agent architectures** — Simple agents often outperform complex ones
- **Large models on small data** — Overfitting is inevitable without regularization

### Surprising Findings
- **DDIM converges faster** but needs more steps for quality
- **Smaller LoRA rank** sometimes outperforms larger ones
- **TF-IDF + SVM** often beats deep learning for text classification
- **Prompt engineering** matters more than model size for many tasks

---

## Architecture (Per Experiment)

Each experiment follows this structure:

```
┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│    Data      │───▶│   Model      │───▶│  Evaluation  │
│  (Raw/Proc)  │    │  (Training)  │    │  (Metrics)   │
└──────────────┘    └──────────────┘    └──────────────┘
       │                   │                   │
       ▼                   ▼                   ▼
  ┌──────────┐       ┌──────────┐       ┌──────────┐
  │  EDA     │       │  WandB   │       │  Plots   │
  │  Plots   │       │  Logging │       │  Reports │
  └──────────┘       └──────────┘       └──────────┘
```

---

## Running Locally

```bash
# Clone
git clone https://github.com/neonite-rc/ml-engineering-lab.git
cd ml-engineering-lab

# Run a specific experiment
cd 06_Build_Your_First_RAG_System_From_Scratch
pip install -r requirements.txt
python rag_from_scratch.py
```

---

## Reproducibility

- All experiments have pinned `requirements.txt`
- Random seeds set for reproducibility
- Cleared outputs for clean notebooks
- Dockerfiles where applicable

---

## License

MIT
