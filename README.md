# ML Engineering Lab

30 independent ML experiments spanning generative AI, NLP, computer vision, MLOps, and agentic systems. Built from scratch during BCA (2023–2026) to understand how these systems work under the hood before applying them at scale.

> **Next step:** Production systems in [ml-systems](https://github.com/neonite-rc/ml-systems).

---

## Problem Statement

I didn't understand how diffusion models actually work. I could use them, but I couldn't explain the noise scheduling, the U-Net architecture, or why DDIM converges faster than DDPM. This repo exists because I learn by building, not by reading.

---

## Experiment Table

| Topic | Hypothesis | Result | Key Learning |
|-------|-----------|--------|-------------|
| Multi-Document RAG | Chunking strategy affects retrieval quality | True | 512 tokens with 50 overlap works best |
| CLIP Alignment | Text-image alignment needs contrastive loss | True | Negative samples matter more than positive |
| Agentic RAG | Router improves retrieval relevance | True | Metadata filtering beats semantic search |
| RAG Pipeline | Context window size affects answer quality | Partially | Diminishing returns beyond 4K tokens |
| Synthetic Medical GANs | GANs can generate realistic medical records | True | Mode collapse is the real problem |
| RAG From Scratch | Custom retrieval beats LangChain default | True | Understanding the code matters |
| Data Augmentation | LLMs can augment training data effectively | True | Quality filtering is critical |
| Data Cleaning | Pandas pipeline beats manual cleaning | True | Reproducibility is the real benefit |
| Data Preprocessing | Python preprocessing is slow | True | Rust gives 10x speedup |
| Docker ML | Containerization improves reproducibility | True | Multi-stage builds save 50% size |
| Diffusion Images | DDPM can generate coherent images | True | 1000 steps is overkill, 500 suffices |
| Document Analysis | LLMs can extract structured info from docs | True | Prompt engineering matters most |
| Fine-tuning | LoRA is efficient for domain adaptation | True | Rank 8 outperforms rank 4 |
| Geospatial Clustering | DBSCAN works for location data | True | Epsilon parameter is dataset-dependent |
| Hybrid ML | Ensemble beats single model | True | Diversity matters more than size |
| MLOps Airflow | Airflow simplifies pipeline management | True | DAG design is the hard part |
| Time Series | Multivariate forecasting beats univariate | True | Feature selection is critical |
| News Collection | Web scraping is fragile | True | Respect robots.txt, add delays |
| Synthetic Data | GANs can generate tabular data | True | Privacy implications are real |
| Text Classification | Simple models often beat complex ones | True | TF-IDF + SVM is surprisingly good |
| Summarization | LLMs summarize better than extractive methods | True | But they hallucinate |
| Automated Cleaning | Rust preprocessing is worth the effort | True | 10x speedup for large datasets |
| Topic Modeling | BERTopic beats LDA | True | But needs more compute |
| Multi-Agent | Agents can collaborate effectively | True | Coordination is the hard part |
| Research Agent | Agents can automate research | Partially | Hallucination is a real problem |
| Game AI | DQN can learn to play games | True | Reward shaping is critical |
| Real-Time AI | RAG can work in real-time | True | Latency is the bottleneck |
| Diffusion From Scratch | U-Net + noise scheduler works | True | Convergence takes patience |
| CrewAI Agents | Multi-agent frameworks simplify development | True | But abstraction leaks |
| LLM From Scratch | Transformer decoder can be trained | True | Data quality matters more than size |

---

## Projects Worth Reading

| Project | Why It Stands Out |
|---------|-------------------|
| RAG From Scratch | Custom retrieval engine without LangChain — proves I understand vector search |
| MLOps with Airflow | Full DAG with model registry, not just a single script |
| Automated Data Cleaning (Rust) | Deliberate language choice for performance-critical preprocessing |
| LLM From Scratch | Transformer decoder trained on custom corpus, not Shakespeare |
| Diffusion Model from Scratch | U-Net + noise scheduler, converged to generate coherent images |

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
cd Build_Your_First_RAG_System_From_Scratch
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
