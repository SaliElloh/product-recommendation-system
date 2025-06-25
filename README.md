# 📚 Collaborative Filtering Recommender System

This project implements and evaluates collaborative filtering approaches to build a personalized recommendation system for health and personal care products using Amazon's 2023 Review dataset. It explores both user-based and item-based collaborative filtering strategies and provides an in-depth comparative analysis of their recommendation quality.

---

## 🔧 Project Structure

### 1. `Collaborative_filtering_Approach1.ipynb`
Implements **User-Based Collaborative Filtering (UBCF)**:
- Filters: Ratings > 4 and users who rated more than 10 items.
- Computes user-user similarity using cosine distance.
- Predicts ratings using K-Nearest Neighbors (KNN).
- Evaluated using RMSE and recommendation statistics.

### 2. `Collaborative_filtering_Approach2.ipynb`
Implements **Item-Based Collaborative Filtering (IBCF)**:
- Filters: Ratings > 3 and users who rated more than 3 items.
- Computes item-item similarity using Pearson correlation.
- Uses KNN from the `surprise` library to predict ratings.
- Evaluated with RMSE and distribution analysis.

### 3. `Analysis_of_recommended_items.ipynb`
Provides analysis of recommended items:
- Evaluates diversity, novelty, popularity.
- Compares item overlaps, distributions, and predictive patterns.
- Includes scatterplots, correlation matrices, and statistical summaries.

---

## 🧠 Methodology

Our approach includes the following steps:

1. **Data Loading & Merging**  
   The dataset was obtained from Amazon (2023) for Health and Personal Care. Review and metadata files were merged after conversion from JSON to Pandas DataFrames.

2. **Data Preprocessing**  
   - Verified integrity and structure.
   - Formed a user-item matrix with 461.7K users, 60.3K items, and 494.1K ratings.

3. **Data Filtering Strategies**  
   - **Approach 1**: Retained ratings > 4 and users with > 10 ratings.
   - **Approach 2**: Retained ratings > 3 and users with > 3 ratings.
   - Additional filtering: Items with fewer than 4 ratings removed (Approach 1), users who rated < 10 items removed (Approach 2).

4. **Model Implementation**  
   - **Machine Learning Model (KNN - Surprise)**:  
     - Input: rating scale (1–5), training/test split (70/30).
     - Similarity: Pearson baseline.
     - Output: RMSE (Approach 1: 0.156, Approach 2: 0.2556).
   - **Traditional Item-Item Collaborative Filtering**:  
     - Each item treated as a vector in high-dimensional space.
     - Pearson correlation used to find most similar items for prediction.

---

## 📊 Experimental Discussion

- **Dataset Size**:  
  - Reviews file: 66.9 MB  
  - Metadata: 22.5 MB  

- **Reduction Summary**:

| Step                  | Approach 1                           | Approach 2                           |
|-----------------------|--------------------------------------|--------------------------------------|
| Rating Filter         | -40% → 301,713 records               | -20% → 369,758 records               |
| User/Item Filter      | -99.5% → 1,522 users                 | -97.7% → 8,767 users                 |

- **Recommendation Output**:
  - Approach 1: 596 items, avg. rating = 4.4, std = 0.2
  - Approach 2: 2,995 items, avg. rating = 4.0, std = 0.48
  - All items from Approach 1 also appeared in Approach 2.
  - MAE = 1.35%, Correlation Coefficient = 0.9021

- **Insights**:
  - Approach 1 offers more precise recommendations.
  - Approach 2 provides a broader set of items, including lower-rated ones.
  - Higher data reduction does not always lead to better performance.

---

## 🧪 Results Summary

- **Approach 1** (user-based): Lower RMSE, more focused, higher predicted ratings.
- **Approach 2** (item-based): Broader coverage, higher variance, slightly less accurate.
- **Overlap**: Complete inclusion of Approach 1 items in Approach 2.

---

## 👩‍💻 Contributions

| Name       | Contributions                                                                 |
|------------|--------------------------------------------------------------------------------|
| **Sali**   | Recommender system design, comparative analysis, documentation                |
| **Samar**  | Data exploration, recommender system, integration, documentation              |
| **Fathi**  | Data loading, cleaning, preprocessing, documentation                          |
| **Yeshaswi** | Data processing, presentation prep, documentation                            |

---

## 📦 Requirements

Install dependencies via:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn scikit-surprise
