# Anime Recommendation System using Databricks

## Overview
This project implements an **end-to-end Anime Recommendation System** using the **Databricks Lakehouse architecture**.  
It demonstrates how real-world recommendation systems are designed using scalable data engineering practices and machine learning techniques.

The system focuses on **four core tasks**:
1. Clean the genre column (handle multiple genres per row)
2. Build a Content-Based Recommendation System (using Genre and Type)
3. Compare with a Collaborative Filtering approach (User Ratings)
4. Recommend the top 5 anime for a given preference (e.g., *Naruto*)

---

## Problem Statement
With thousands of anime titles available across genres and formats, users often struggle to discover content that matches their interests.  
Simple popularity-based recommendations fail to personalize suggestions.

This project addresses that challenge by building:
- A **Content-Based Recommender** (based on anime attributes)
- A **Collaborative Filtering Recommender** (based on user ratings)

Both approaches are implemented, compared, and deployed using Databricks.

---

## Datasets

### Anime Dataset
- **Rows:** 12,294  
- **Columns:** 7  

| Column Name | Description |
|------------|-------------|
| anime_id | Unique anime identifier |
| name | Anime title |
| genre | Comma-separated genres |
| type | TV, Movie, OVA, etc. |
| episodes | Episode count |
| rating | Average rating |
| members | Community members count |

---

### Rating Dataset
- **Rows:** 7,813,737  
- **Columns:** 3  

| Column Name | Description |
|------------|-------------|
| user_id | Unique user identifier |
| anime_id | Anime watched |
| rating | User rating (-1 means watched but not rated) |

---

## Architecture: Databricks Lakehouse

The project follows a **Bronze → Silver → Gold** architecture.

### Bronze Layer
- Raw datasets uploaded as-is
- No transformations
- Source of truth

---

### Silver Layer
Data cleaning and standardization.

#### Anime Dataset Cleaning
- Removed special characters and unwanted symbols from anime names
- Standardized anime name capitalization
- Replaced null genres with `"Unknown"`
- Converted genre column into arrays
- Removed duplicate genres per anime
- Replaced null values in `type` with `"Unknown"`
- Filled null ratings with `0.0`

Saved as a **Delta table** in the Silver layer.

#### Rating Dataset Cleaning
- No null values
- `rating = -1` replaced with `0` (watched but not rated)
- Duplicate rows preserved (valid user behavior)
- Standardized column names

Saved as a **Delta table** in the Silver layer.

---

## Task 1: Clean the Genre Column
- Split multi-valued genre strings into arrays
- Removed duplicate genres within a single anime
- Standardized genre representation for downstream modeling

---

## Task 2: Content-Based Recommendation System

### Approach
- Combined `genre` and `type` into a unified **content feature**
- Converted both fields into array format
- Vectorized content features
- Computed similarity using **cosine similarity**

### Key Feature
- The system is **not limited to Naruto**
- Admin/user can input **any anime name**
- The system returns **top 5 similar anime**

This approach focuses purely on **content similarity**, independent of user behavior.

---

## Task 3: Collaborative Filtering (User Ratings)

### Approach
- Implemented using **Python, Pandas, NumPy, and cosine similarity**
- Recommendations based on users with similar rating patterns
- Anime highly rated by similar users are recommended

### Assumptions & Limitations
- High ratings imply user preference
- Can be noisy for users with sparse ratings
- Relies fully on historical user behavior

This model was implemented **outside PySpark** for better control and memory efficiency.

---

## Task 4: Final Recommendations
- **Content-Based:**  
  Input an anime name → Get top 5 similar anime
- **Collaborative Filtering:**  
  Input a user ID → Get anime recommendations with anime name and average rating

---

## Pipeline Execution
- Bronze → Silver → Gold notebooks executed sequentially
- Delta tables created successfully
- Recommendation outputs generated without errors

---

## Tools & Technologies
- Databricks
- Apache Spark (PySpark)
- Delta Lake
- Python
- Pandas
- NumPy
- Scikit-learn

---

## Key Learnings
- Designing scalable recommender systems
- Implementing Lakehouse architecture
- Handling large-scale data cleaning
- Comparing content-based vs collaborative filtering
- Building production-style data pipelines

---

## Future Enhancements
- Hybrid recommendation model
- Model evaluation metrics (Precision@K, Recall@K)
- Real-time recommendation API
- User feedback loop

---

## Author
**Rohini Singh**
