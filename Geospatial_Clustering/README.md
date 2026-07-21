# Geospatial Clustering

K-Means and DBSCAN clustering on delivery location data with outlier detection, zone labeling, cluster profiling, route optimization (TSP), and interactive map visualization.

---

## Problem Statement

Delivery companies have hundreds of drop-off points scattered across a city. How do you automatically group them into zones, identify outliers, profile each zone's delivery density, and optimize routes within each zone — all from raw latitude/longitude coordinates?

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Clustering** | K-Means + DBSCAN comparison | K-Means for defined zones; DBSCAN for automatic outlier detection and irregular cluster shapes |
| **Optimal K** | Elbow method (within-cluster sum of squares) | Visual and quantitative — the elbow at k=3 clearly separated the data into 3 natural zones |
| **Outlier detection** | IQR on distance-to-centroid | Points far from their cluster center are outliers — more principled than arbitrary distance thresholds |
| **Zone labeling** | Coordinate-based heuristic (lat/lng thresholds) | Maps cluster IDs to human-readable names: "Downtown/Financial District", "Uptown/North Zone" |
| **Route optimization** | Nearest-neighbor TSP heuristic | O(n²) but fast enough for 10-50 points per zone; produces reasonable routes without exact TSP solvers |
| **Visualization** | Matplotlib (static) + Folium (interactive) | Static plots for reports; interactive maps for exploration |

---

## What I Learned

- **Scaling coordinates before K-Means is essential.** Latitude ranges ~0.1° while longitude ranges ~0.05°. Without StandardScaler, K-Means treats latitude as 2x more important — producing distorted clusters.
- **DBSCAN and K-Means disagree — and that's useful.** K-Means forced every point into a cluster. DBSCAN identified noise points that K-Means misclassified. Comparing both gives a fuller picture.
- **Cluster profiling turns coordinates into business insights.** Adding density (deliveries per km²) and centroid location transforms abstract cluster IDs into "Uptown has 35 deliveries across 12 km² — our densest zone."
- **Nearest-neighbor TSP is good enough.** For zone-level routing (10-50 stops), the nearest-neighbor heuristic produces routes within 20% of optimal. Exact TSP solvers add complexity without proportional improvement.

---

## Key Insight

> "Clustering coordinates is the easy part. The value comes from what you do after: labeling zones, profiling density, detecting outliers, and optimizing routes within each cluster."

---

## Running

```bash
pip install pandas numpy matplotlib scikit-learn folium
python geospatial_clustering.py
```
