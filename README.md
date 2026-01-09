# Sample Applied Data Analysis 
### Preparation for CS-401 Exam

This notebook is a **sample end-to-end applied data analysis** conducted on a Reddit posts + comments dataset.  Available at: [Kaggle – The Reddit Dataset](https://www.kaggle.com/datasets/pavellexyr/the-reddit-dataset-dataset)

The goal is to practice exam-level workflows, methods, and reasoning. There is no focus on optimizing performance.

---

## Dataset
- Reddit posts and comments from r/datasets
- Two tables:
  - `posts`: post metadata, title, selftext, score, timestamp
  - `comments`: comment text, sentiment, score, permalink
- **Limitations**:
  - No author IDs
  - No reply structure
  - No explicit user interaction data

---

## 1. Data Preparation
- Constructed a **document per post** by concatenating:
  - post title + selftext
  - all comments belonging to the post
- Fixed merge issues by:
  - extracting post IDs from comment permalinks
  - merging on post ID instead of full URLs
- Created:
  - `document`: raw text
  - `document_clean`: ASCII-only, alphabetic, lowercased text

---

## 2. Exploratory Analysis & Statistics
- Examined outcome variable: `score`
- Observed **strong right-skew** in:
  - post scores
  - document lengths
- Created linguistic feature:
  - `doc_length`
- Performed **group comparison**:
  - High vs low score posts
  - Threshold defined using **geometric mean** (robust to skew)
- Conducted:
  - Welch’s t-test
  - Repeated test after IQR-based outlier removal
- Visualized results using:
  - histograms
  - mean + 95% confidence intervals (CI)

---

## 3. Text Representation (NLP)
- Built **TF-IDF representations**:
  - `max_features`
  - English stopword removal
- Visualized high-dimensional text using **Truncated SVD (2D)**
- Interpreted overlap vs separation cautiously
- Addressed document-length bias by:
  - applying a **log-based length decay** to TF-IDF vectors

---

## 4. Predictive Modeling
- Defined binary outcome:
  - high vs low score (geometric mean threshold)
- Trained **logistic regression** models:
  - standard TF-IDF
  - length-decayed TF-IDF
- Evaluated using:
  - accuracy
  - majority-class baseline
- Interpreted model coefficients:
  - top positive and negative terms

---

## 5. Network Analysis (Iterative Refinement)

### 5.1 Word Co-occurrence Network
- Built word–word co-occurrence networks
- Identified key issues:
  - stopword dominance
  - full connectivity
- Fixed by:
  - stopword removal
  - thresholding
- Applied:
  - degree
  - PageRank
  - betweenness centrality

---

### 5.2 Post–Post Similarity Network (Final Network)
- Constructed a **semantic similarity network**:
  - Nodes: posts
  - Edges: cosine similarity of TF-IDF vectors
  - Thresholded to avoid full connectivity
- Analyzed:
  - connected components
  - degree and weighted degree
  - PageRank (influential posts)
  - betweenness (bridge posts)
- Visualized:
  - largest connected component
- Retrieved and inspected **actual post content** for top-central nodes

---

## Libraries Practiced
- pandas, numpy
- seaborn, matplotlib
- scipy.stats
- scikit-learn (TF-IDF, SVD, logistic regression)
- networkx
- itertools, collections

---

## Purpose
This notebook serves as a reference implementation and conceptual checklist for the CS-401 (Fall 2025) Applied Data Analysis exam.
