# Collaborative Filtering Recommendation System 🛍️🤖

[![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)](https://python.org)
[![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)](https://pandas.pydata.org)
[![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat&logo=numpy&logoColor=white)](https://numpy.org)
[![Scikit-learn](https://img.shields.io/badge/Scikit--learn-F7931E?style=flat&logo=scikit-learn&logoColor=white)](https://scikit-learn.org)
[![University of Michigan](https://img.shields.io/badge/UMich-00274C?style=flat)](https://umich.edu)

> University of Michigan, Dearborn | Winter 2024

---

## Overview

This project builds and evaluates a **personalized product recommendation system** for health and personal care products using the Amazon 2023 Reviews dataset. It implements and compares two collaborative filtering strategies — **User-Based (UBCF)** and **Item-Based (IBCF)** — analyzing their tradeoffs in accuracy, coverage, and recommendation diversity at scale.

The full Amazon 2023 Reviews dataset spans **10M+ interactions** across all product categories. This project loads and processes the complete dataset before filtering to the Health & Personal Care domain, providing a realistic large-scale data engineering challenge on top of the modeling work.

---

## Results

| Metric | Approach 1 (UBCF) | Approach 2 (IBCF) |
|---|---|---|
| Similarity Method | Cosine (user-user) | Pearson correlation (item-item) |
| Rating Filter | > 4 stars | > 3 stars |
| Users retained | 1,522 | 8,767 |
| Items recommended | 596 | 2,995 |
| Avg. predicted rating | 4.4 ± 0.2 | 4.0 ± 0.48 |
| **RMSE** | **0.156** | **0.2556** |
| Correlation (UBCF vs IBCF) | 0.9021 | — |
| MAE between approaches | 1.35% | — |

**Key finding:** All 596 items recommended by Approach 1 (UBCF) appeared in Approach 2 (IBCF), confirming that stricter filtering produces a high-precision subset of the broader recommendation space.

---

## Dataset

**Amazon Product Reviews 2023** — one of the largest publicly available e-commerce interaction datasets.

| File | Size | Scope |
|---|---|---|
| Reviews (raw, all categories) | 66.9 MB | **10M+ interactions** |
| Metadata | 22.5 MB | Product info |
| Health & Personal Care subset | — | 494,100 ratings, 461.7K users, 60.3K items |

Data source: [Amazon Reviews 2023](https://amazon-reviews-2023.github.io/)

---

## Methodology

### Pipeline Overview

```
Full Amazon 2023 Dataset (10M+ interactions)
           │
           ▼
  Filter: Health & Personal Care
           │
           ▼
  User-Item Matrix (494.1K ratings)
     461.7K users × 60.3K items
           │
     ┌─────┴─────┐
     ▼           ▼
  Approach 1   Approach 2
  (UBCF)       (IBCF)
  Cosine sim   Pearson corr
  KNN          KNN (Surprise)
  RMSE: 0.156  RMSE: 0.256
```

### Data Preprocessing

- Loaded raw JSON review and metadata files and converted to Pandas DataFrames
- Verified data integrity and formed a sparse user-item interaction matrix
- Applied two filtering strategies to study the precision-coverage tradeoff:

| Step | Approach 1 (UBCF) | Approach 2 (IBCF) |
|---|---|---|
| Rating filter | > 4 stars → 301,713 records (−40%) | > 3 stars → 369,758 records (−20%) |
| User/item filter | Users with > 10 ratings → 1,522 users (−99.5%) | Users with > 3 ratings → 8,767 users (−97.7%) |

### Approach 1 — User-Based Collaborative Filtering (UBCF)

Finds users with similar rating patterns to the target user, then recommends products those similar users rated highly.

- Similarity metric: **Cosine distance** between user rating vectors
- Prediction: **K-Nearest Neighbors (KNN)**
- Train/test split: 70/30
- Result: High-precision recommendations with low variance (avg rating 4.4 ± 0.2)

### Approach 2 — Item-Based Collaborative Filtering (IBCF)

Treats each item as a vector in user-rating space, finds similar items to those a user has rated, and recommends them.

- Similarity metric: **Pearson correlation** between item vectors
- Prediction: **KNN from the Surprise library**
- Result: Broader coverage with higher variance (avg rating 4.0 ± 0.48)

---

## Key Findings

- **UBCF achieves lower RMSE (0.156 vs 0.256)** — stricter filtering creates a more consistent user preference signal
- **IBCF provides 5× more recommendations** (2,995 vs 596 items) — better for discovery and diversity
- **High correlation (r = 0.9021)** between both approaches' predictions — both methods converge on similar quality assessments
- **Complete inclusion**: every item UBCF recommends also appears in IBCF, confirming UBCF is a high-confidence subset
- **Higher data reduction ≠ better performance** — Approach 2's looser filter captures more signal despite retaining more noise

---

## Project Structure

```
Collaborative-Filtering-Recommender/
├── Collaborative_filtering_Approach1.ipynb   # UBCF: cosine KNN
├── Collaborative_filtering_Approach2.ipynb   # IBCF: Pearson KNN (Surprise)
├── Analysis_of_recommended_items.ipynb       # Diversity, novelty, overlap analysis
└── README.md
```

---

## Getting Started

### Prerequisites

```bash
pip install pandas numpy scikit-learn scikit-surprise matplotlib seaborn
```

### Run the notebooks

```bash
# Clone the repository
git clone https://github.com/SaliElloh/amazon-recommendation-system
cd amazon-recommendation-system
```

1. Download the Amazon 2023 Health & Personal Care dataset from [amazon-reviews-2023.github.io](https://amazon-reviews-2023.github.io/)
2. Place the `reviews` and `metadata` JSON files in the root directory
3. Open and run the notebooks in order: Approach 1 → Approach 2 → Analysis

> **Note:** The full Amazon 2023 dataset is large. A machine with at least 8GB RAM is recommended for the preprocessing step.

---

## Contributors

| Name | Contributions |
|---|---|
| **Sali El-loh** | Recommender system design, comparative analysis, documentation |
| **Samar** | Data exploration, recommender system, integration, documentation |
| **Fathi** | Data loading, cleaning, preprocessing, documentation |
| **Yeshaswi** | Data processing, presentation prep, documentation |

---

## Author

**Sali El-loh**
M.S. Artificial Intelligence | University of Michigan — Dearborn
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/salielloh12/)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=flat&logo=github&logoColor=white)](https://github.com/SaliElloh)
[![Email](https://img.shields.io/badge/Email-D14836?style=flat&logo=gmail&logoColor=white)](mailto:selloh@umich.edu)

---

## License

No license specified. Contact the author for usage permissions.
