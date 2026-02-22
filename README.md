# Unsupervised Clustering of MBTI Personality Types from Text

This project investigates whether personality-related structure can be recovered from textual data using **unsupervised learning**, without relying on personality labels during training. We focus on distinguishing between **ENFP** and **INTJ** personality types using forum posts and evaluate two clustering algorithms across multiple feature representations.

Using TF-IDF and handcrafted linguistic features combined with dimensionality reduction, we evaluate **K-Medoids** and **Affinity Propagation** clustering. The best-performing configuration achieves **77% cluster purity**, approaching supervised baselines trained on the same data, demonstrating that meaningful personality-related patterns can be partially recovered from text without supervision.

---

## Dataset

The experiments use the *Myers–Briggs Personality Type Dataset*, consisting of forum posts labeled with MBTI types.

- Each sample contains the last **50 forum posts** written by a user  
- Only **ENFP** and **INTJ** users are retained  
- The dataset is **balanced via undersampling**
- Final size: **675 samples per class**

**Data source:** Myers–Briggs Personality Type Dataset, Kaggle 
https://www.kaggle.com/datasets/datasnaek/mbti-type

---

## Methods

### Feature Representations
Two complementary representations are used:
- **TF-IDF**: 1,000 most informative unigrams
- **Handcrafted linguistic features**: punctuation usage, sentence length, vocabulary complexity, expressive markers

### Dimensionality Reduction
To reduce noise and improve clustering:
- PCA  
- Truncated SVD  
- UMAP  
(with 2, 5, and 10 components)

### Clustering Algorithms
- **K-Medoids** (k = 2) — robust, medoid-based clustering  
- **Affinity Propagation** — automatically discovers the number of clusters

---

## Evaluation

Clustering quality is assessed using three complementary metrics:
- **Purity**
- **Adjusted Rand Index (ARI)**
- **Silhouette Score**

These are combined into a single score:

\[
\text{Combined Score} = 0.4 \cdot \text{Purity} + 0.4 \cdot \text{ARI} + 0.2 \cdot \text{Silhouette}
\]

This weighting balances label alignment, chance correction, and geometric separation.

---

## Results

### Best Test-Set Performance

| Method | Features | Purity | ARI |
|------|---------|--------|-----|
| **K-Medoids** | TF-IDF + SVD (5) | **0.77** | **0.29** |
| Affinity Propagation | TF-IDF + SVD (5) | 0.73 | 0.18 |
| Random Baseline | — | 0.52 | 0.00 |

Key observations:
- TF-IDF features consistently outperform handcrafted features
- K-Medoids produces clusters more aligned with personality labels
- Unsupervised performance approaches supervised baselines (≈79% accuracy)

---

## Cluster Interpretation

Despite the inherent ambiguity of inferring personality from text, the discovered clusters exhibit meaningful stylistic differences:

- **INTJ-dominant cluster**
  - Higher lexical complexity
  - More analytical and structured language
  - Fewer expressive punctuation markers

- **ENFP-dominant cluster**
  - Increased use of exclamation marks and expressive punctuation
  - Informal and socially oriented vocabulary

Misclustered samples often display mixed stylistic traits, suggesting that personality expression in text lies on a continuum rather than forming strictly separable categories.

---
