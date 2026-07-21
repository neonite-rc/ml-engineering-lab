# Real-Time News Data Collection

An R-based pipeline that fetches, stores, and analyzes news articles with sentiment scoring, trend detection, and an interactive Shiny dashboard.

---

## Problem Statement

Raw news data is scattered across APIs, hard to deduplicate, and impossible to analyze without structured storage. This project tackles the full pipeline: API ingestion, persistent SQLite storage, sentiment analysis, anomaly detection, and visualization — all in R.

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Language** | R (tidyverse ecosystem) | `tm` and `wordcloud` packages are mature for text mining; `RSQLite` makes persistence trivial |
| **Storage** | SQLite with duplicate detection | Lightweight, file-based, no server needed. URL-based deduplication prevents re-inserting same articles |
| **Sentiment** | Lexicon-based scoring (positive/negative word lists) | Fast, interpretable, no model loading. Good enough for relative comparison between sources |
| **Trend detection** | Document-term matrix + term frequency | Classic NLP approach — effective for identifying emerging topics without model inference |
| **Anomaly detection** | Z-score on sentiment values | Simple threshold-based approach that catches articles with unusually positive/negative tone |
| **Fallback** | Local JSON + dummy data | Works without an API key — the pipeline degrades gracefully |

---

## What I Learned

- **API keys are the first point of failure.** The pipeline gracefully falls back to dummy data when no `NEWS_API_KEY` is set, which is essential for reproducible demos.
- **Lexicon-based sentiment is surprisingly useful.** For comparing news sources (e.g., "which source is most positive?"), a simple word-list approach is fast and interpretable — you don't always need BERT.
- **Pipeline observability matters.** The logging system (INFO/WARNING/ERROR with timestamps) and collection_log table turned debugging from "what went wrong?" to "exactly what failed and when."
- **Shiny dashboards make analysis accessible.** A non-technical user can filter news by source, view word clouds, and explore sentiment trends without touching R code.

---

## Key Insight

> "Data collection isn't the glamorous part of ML, but a pipeline that can't reliably collect, deduplicate, and store data isn't a pipeline — it's a script that runs once."

---

## Running

```bash
# Set API key (optional — works without it using dummy data)
export NEWS_API_KEY="your_api_key_here"

# Run the pipeline
Rscript news_collection.R

# Run the dashboard
Rscript -e 'shiny::runApp("app.R")'
```
