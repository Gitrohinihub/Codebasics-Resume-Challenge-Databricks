# Anime Recommendation System using Databricks

## Overview
This project implements an **end-to-end Anime Recommendation System** using the **Databricks Lakehouse architecture**.  
It demonstrates how real-world recommendation systems are designed using scalable data engineering practices and machine learning techniques.

The system focuses on **four core tasks**:
1. Clean the genre column (handle multiple genres per row)
2. Build a Content-Based Recommendation System (using Genre and Type)
3. Recommend the top 5 anime for a given preference (e.g., *Naruto*)

---

## Problem Statement
With thousands of anime titles available across genres and formats, users often struggle to discover content that matches their interests.  
Simple popularity-based recommendations fail to personalize suggestions.

This project addresses that challenge by building:
- A **Content-Based Recommender** (based on anime attributes)

Both approaches are implemented, compared, and deployed using Databricks.
---

## Datasets
The dataset is from the PromptBI Website.

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

![](https://github.com/Gitrohinihub/Codebasics-Resume-Challenge-Databricks/blob/1c92a5c58dbd9d6c761c111bcc36ed8a82b6b6ae/dataset.png)
---

## Architecture: Databricks Lakehouse

The project follows a **Bronze → Silver → Gold** architecture.

![](https://github.com/Gitrohinihub/Codebasics-Resume-Challenge-Databricks/blob/1c92a5c58dbd9d6c761c111bcc36ed8a82b6b6ae/Catalog_schema_layers.png)

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

### Gold Layer 

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

## Task 3: Final Recommendations
- **Content-Based:**  
  Input an anime name → Get top 5 similar anime

---

## Pipeline Execution
- Bronze → Silver → Gold notebooks executed sequentially
- Delta tables created successfully
- Recommendation outputs generated without errors

![](https://github.com/Gitrohinihub/Codebasics-Resume-Challenge-Databricks/blob/1c92a5c58dbd9d6c761c111bcc36ed8a82b6b6ae/pipeline%20run.png)
---

## Tools & Technologies
- Databricks
- Apache Spark (PySpark)
- Delta Lake
- ML Lib

---

## Key Learnings
- Designing scalable recommender systems
- Implementing Lakehouse architecture
- Handling large-scale data cleaning
- Building production-style data pipelines

---

## project Owner
**Rohini Singh**
Linkedin : [click here](https://www.linkedin.com/in/rohini-singh-)
presentation : [click here](https://www.linkedin.com/posts/rohini-singh-_codebasicsresumechallenge-databricks-codebasics-activity-7423062901084635136-adJI?utm_source=social_share_send&utm_medium=android_app&rcm=ACoAADk_WcEBndGed7gzDwaG8CNJocWgQnSTThQ&utm_campaign=copy_link)
