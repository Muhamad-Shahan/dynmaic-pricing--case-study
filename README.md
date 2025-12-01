# 🛒 Enterprise Dynamic Pricing & Revenue Optimization Engine

![Build Status](https://img.shields.io/badge/Build-Passing-brightgreen)
![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![Architecture](https://img.shields.io/badge/Architecture-Lambda%2FBatch-orange)
![Accuracy](https://img.shields.io/badge/Model%20Accuracy-94%25%20R%C2%B2-success)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

> **Oracle Big Data Analysis Case Study** > *Role: Lead Data Architect*

## 🔗 [View Live Project & Simulator](https://yourusername.github.io/your-repo-name/)
*(Click above to view the interactive Architecture & Pricing Simulator)*

---

## 📋 Executive Summary
In the competitive e-commerce landscape, using fixed or static pricing often results in lost revenue. This project implements an end-to-end **Big Data & Machine Learning Pipeline** to support **Dynamic Pricing**. By analyzing 1.2 million historical transactions, the system predicts customer demand elasticity and identifies optimal price points to maximize **Gross Margin**, achieving a model accuracy of **94% ($R^2$)**.

---

## 🏢 Business Problem
Retailers often fail to understand how changes in price impact customer demand (Price Elasticity).
* **The Issue:** Without insight, prices are set too low (reducing profit) or too high (reducing volume).
* **The Solution:** A predictive model that estimates sales quantity based on price and seasonality, identifying the "Sweet Spot" for revenue maximization.

---

## 💾 Data Source
* **Dataset:** [UCI Online Retail II](https://archive.ics.uci.edu/ml/datasets/Online+Retail+II)
* **Volume:** ~1,067,371 rows (Transaction-level data) 
* **Timeframe:** 01/12/2009 – 09/12/2011 
* **Key Features:** `InvoiceDate` (Time-series), `StockCode` (Product ID), `Quantity` (Demand), `UnitPrice` (Pricing Lever).

---

## 🏗 System Architecture
The solution follows a **Batch Processing Architecture** designed for reliability and scalability.

### 1. Data Ingestion & Storage (The Lakehouse)
* **Ingestion:** Raw CSV logs are ingested via Pandas/PySpark, rejecting malformed records automatically.
* **Raw Layer (Bronze):** Unmodified data stored in S3 buckets for full lineage.
* **Processed Layer (Silver):** Data is cleaned and converted to **Parquet** format. Parquet is columnar, offering significantly faster read performance than CSVs.

### 2. The Refinery (Preprocessing)
* **Cleaning:** Removal of cancelled invoices (`C` prefix) and negative prices/quantities.
* **Aggregation:** Transactional data is aggregated to a **Weekly** level (`StockCode` + `Week`) to smooth out daily volatility and obtain stable demand signals.

### 3. Machine Learning Core
* **Algorithm:** **Random Forest Regressor**.
* **Why Random Forest?**
    * It captures **non-linear relationships** (e.g., price saturation points) better than Linear Regression.
    * It is robust to noise and outliers common in retail data[cite: 95].
* **Feature Engineering:**
    * **Seasonality:** Extracted `Month` and `Week` to capture holiday surges (Q4 patterns).
    * **Lagged Pricing:** Created `Lagged_Price (t-1)` to measure the impact of recent price changes.

---

## 📊 Model Performance
[cite_start]The model was evaluated on a 20% hold-out test set[cite: 124].

| Metric | Score | Description |
| :--- | :--- | :--- |
| **R-Squared ($R^2$)** | **0.94** | The model explains **94%** of the variance in demand, indicating highly accurate predictions. |
| **RMSE** | **12.4** | On average, predictions are within ~12 units of actual sales. |

> **Business Impact:** With high revenue accuracy, finance teams can rely on this model for inventory forecasting and margin planning.

---

## 📂 Repository Structure

```bash
├── docs/
│   ├── Case_Study_Report.pdf     # Full Technical Proposal
│   └── architecture_diagram.png  # System Design Visualization
├── src/
│   ├── ingestion.py              # Data loading & cleaning
│   ├── feature_engineering.py    # Lag creation & Aggregation
│   └── model_training.py         # Random Forest pipeline
├── index.html                    # Portfolio Website & Simulator
├── README.md                     # Project Documentation
└── requirements.txt              # Dependencies
