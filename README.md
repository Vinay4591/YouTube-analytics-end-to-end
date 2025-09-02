# 🎬 YouTube Analytics Pipeline | AWS + Power BI

An **end-to-end data engineering and analytics pipeline** that ingests YouTube category/region data, processes it using AWS services, and visualizes insights in **Power BI dashboards**.  

This project demonstrates how to build a **cloud-native analytics pipeline** — from raw JSON/CSV data to interactive BI dashboards.

---

## 📌 Architecture Overview

![AWS Architecture]()

**Flow**:  
`YouTube API / CSV → S3 (Raw) → Lambda (JSON → Parquet) → Glue ETL → S3 (Cleaned → Analytics) → Glue Data Catalog → Athena → Power BI`

---

## ⚙️ Pipeline Components

### 🔹 Data Ingestion
- **Sources**: Kaggle (JSON), CSV files  
- **S3 Raw Bucket**: `youtube-analysis-raw-data-us-east-2-dev`  
- **Event Trigger**: S3 → Lambda on file upload  

### 🔹 Data Processing
- **Lambda Function**  
  - Converts JSON → Parquet for efficient querying  
  - Validates schema and adds metadata (region, ingest date)  

- **AWS Glue Jobs**  
  - Cleaning & validation  
  - Transformation & ETL  
  - Automated schema discovery + Data Catalog updates  

### 🔹 Data Storage
- **S3 Cleaned Bucket**: `youtube-analysis-cleaned-data-us-east-2-dev`  
- **S3 Analytics Bucket**: `youtube-analysis-analytics-us-east-2-dev`  
- **S3 Athena Results Bucket**: `youtube-analysis-raw-data-us-east-2-athena`  

### 🔹 Analytics & Querying
- **Athena SQL Queries** on Parquet data (serverless, pay-per-query)  
- Query results stored in dedicated Athena bucket  

### 🔹 Visualization
- **Power BI** dashboards connected to Athena via ODBC driver  
- Interactive reports on views, likes, dislikes, trends, and engagement  

---

## 📊 Dashboards

### 1. Audience & Region Insights
- Regional share of views  
- Best upload times (day/hour)  
- Days to trend vs publish lag  

![Audience Dashboard](https://github.com/Vinay4591/YouTube-analytics-end-to-end/blob/81220efadb0dba6b7ebae4a9203a7a845735daed/PowerBi%20Dashboard/youtube%20analytics%201.png)

---

### 2. Content Performance
- Top 10 trending videos by views  
- Views over time (daily/quarterly trends)  
- Engagement metrics (likes-to-dislike ratio, engagement rate, trending lag)  

![Performance Dashboard](https://github.com/Vinay4591/YouTube-analytics-end-to-end/blob/81220efadb0dba6b7ebae4a9203a7a845735daed/PowerBi%20Dashboard/youtube%20analytics%202.png)

---

## 🚀 Steps to Reproduce

1. **Set up S3 Buckets**  
   - `youtube-analysis-raw-data-us-east-2-dev`  
   - `youtube-analysis-cleaned-data-us-east-2-dev`  
   - `youtube-analysis-analytics-us-east-2-dev`  
   - `youtube-analysis-raw-data-us-east-2-athena`  

2. **Deploy Lambda Function**  
   - Trigger: `s3:ObjectCreated:*` on `raw` bucket  
   - Code: `lambda/lambda_json_to_parquet.py`  

3. **Run AWS Glue Jobs**  
   - Job 1: Cleaning & validation  
   - Job 2: ETL & schema transformation  

4. **Configure Glue Data Catalog**  
   - Run crawler on cleaned/analytics buckets  
   - Create Athena tables from catalog  

5. **Query with Athena**  
   - Run SQL queries in `sql/analytics_queries.sql`  

6. **Connect Power BI to Athena**  
   - Use ODBC/Simba driver  
   - Build dashboards with dataset from Athena  

---

## 🛠️ Tech Stack

- **AWS**: S3, Lambda, Glue, Athena, Glue Data Catalog  
- **ETL**: Python (boto3, pandas, pyarrow)  
- **Visualization**: Power BI  
- **Data Format**: JSON, CSV → Parquet  
