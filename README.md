# Uber NYC — Pickup Hot Zones

Unsupervised Machine Learning project to identify hot zones for Uber drivers in New York City.

## Goal

Uber drivers are not always where users need them. This project uses clustering algorithms to recommend **hot zones** — areas where drivers should position themselves at any given hour and day of the week.

## Data

| File | Period | Rows |
|---|---|---|
| `uber-raw-data-apr14.csv` | April 2014 | ~564k |
| `uber-raw-data-may14.csv` | May 2014 | ~652k |
| `uber-raw-data-jun14.csv` | June 2014 | ~663k |
| `uber-raw-data-jul14.csv` | July 2014 | ~796k |
| `uber-raw-data-aug14.csv` | August 2014 | ~829k |
| `uber-raw-data-sep14.csv` | September 2014 | ~1.03M |

> `uber-raw-data-janjune-15.csv` was excluded — it uses `locationID` instead of GPS coordinates (`Lat`/`Lon`), making it incompatible with the 2014 files without additional geospatial data.

## Algorithms

### KMeans
- Number of clusters fixed by the **Elbow method** (WCSS minimization)
- Validated with **Silhouette score**
- Best for: defining a fixed number of operational zones

### DBSCAN
- Number of clusters detected **automatically** based on density
- Parameters: `eps=0.15`, `min_samples=3`, `metric='manhattan'`
- Handles outliers (noise points labeled `-1`)
- Best for: discovering natural pickup concentrations

## Notebook Structure

| Section | Content |
|---|---|
| 1 | Imports |
| 2 | Single file exploration (apr14) |
| 3 | KMeans — snapshot analysis (day=1, hour=10h) |
| 4 | DBSCAN — same snapshot + KMeans vs DBSCAN comparison |
| 5 | KMeans — generalization hour by hour |
| 6 | KMeans — generalization by day of week (single file) |
| 7 | Loading all 6 CSV files |
| 8 | Global EDA — all files |
| 9 | KMeans hot zones by day of week — all files |
| 10 | DBSCAN — Friday 6pm snapshot — all files |
| 11 | Conclusion |

## Setup

```bash
pip install pandas numpy plotly scikit-learn yellowbrick matplotlib
```

```bash
jupyter notebook uber_project_final.ipynb
```

## Key Results

- Peak demand: **7h–9h** and **17h–19h**
- Busiest days: **Friday and Saturday evenings**
- Dominant zone: **Midtown Manhattan** at all hours
- KMeans optimal k: **4** clusters on a single snapshot
- DBSCAN: **7–12 clusters** detected depending on parameters, ~13% noise
