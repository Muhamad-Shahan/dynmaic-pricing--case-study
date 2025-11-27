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
[cite_start]In the competitive e-commerce landscape, using fixed or static pricing often results in lost revenue[cite: 3]. This project implements an end-to-end **Big Data & Machine Learning Pipeline** to support **Dynamic Pricing**. By analyzing 1.2 million historical transactions, the system predicts customer demand elasticity and identifies optimal price points to maximize **Gross Margin**, achieving a model accuracy of **94% ($R^2$)**.

---

## 🏢 Business Problem
[cite_start]Retailers often fail to understand how changes in price impact customer demand (Price Elasticity)[cite: 4].
* [cite_start]**The Issue:** Without insight, prices are set too low (reducing profit) or too high (reducing volume)[cite: 5].
* [cite_start]**The Solution:** A predictive model that estimates sales quantity based on price and seasonality [cite: 7][cite_start], identifying the "Sweet Spot" for revenue maximization[cite: 8].

---

## 💾 Data Source
* [cite_start]**Dataset:** [UCI Online Retail II](https://archive.ics.uci.edu/ml/datasets/Online+Retail+II) [cite: 10]
* [cite_start]**Volume:** ~1,067,371 rows (Transaction-level data) [cite: 14]
* [cite_start]**Timeframe:** 01/12/2009 – 09/12/2011 [cite: 11]
* [cite_start]**Key Features:** `InvoiceDate` (Time-series), `StockCode` (Product ID), `Quantity` (Demand), `UnitPrice` (Pricing Lever)[cite: 17, 18, 19, 20].

---

## 🏗 System Architecture
[cite_start]The solution follows a **Batch Processing Architecture** designed for reliability and scalability[cite: 25].

### 1. Data Ingestion & Storage (The Lakehouse)
* [cite_start]**Ingestion:** Raw CSV logs are ingested via Pandas/PySpark, rejecting malformed records automatically[cite: 32, 33].
* [cite_start]**Raw Layer (Bronze):** Unmodified data stored in S3 buckets for full lineage[cite: 35].
* **Processed Layer (Silver):** Data is cleaned and converted to **Parquet** format. [cite_start]Parquet is columnar, offering significantly faster read performance than CSVs[cite: 37, 38].

### 2. The Refinery (Preprocessing)
* [cite_start]**Cleaning:** Removal of cancelled invoices (`C` prefix) and negative prices/quantities[cite: 42].
* [cite_start]**Aggregation:** Transactional data is aggregated to a **Weekly** level (`StockCode` + `Week`) to smooth out daily volatility and obtain stable demand signals[cite: 43].

### 3. Machine Learning Core
* [cite_start]**Algorithm:** **Random Forest Regressor**[cite: 48].
* **Why Random Forest?**
    * [cite_start]It captures **non-linear relationships** (e.g., price saturation points) better than Linear Regression[cite: 94].
    * [cite_start]It is robust to noise and outliers common in retail data[cite: 95].
* **Feature Engineering:**
    * [cite_start]**Seasonality:** Extracted `Month` and `Week` to capture holiday surges (Q4 patterns)[cite: 83, 84].
    * [cite_start]**Lagged Pricing:** Created `Lagged_Price (t-1)` to measure the impact of recent price changes[cite: 86].

---

## 📊 Model Performance
[cite_start]The model was evaluated on a 20% hold-out test set[cite: 124].

| Metric | Score | Description |
| :--- | :--- | :--- |
| **R-Squared ($R^2$)** | **0.94** | [cite_start]The model explains **94%** of the variance in demand, indicating highly accurate predictions[cite: 137]. |
| **RMSE** | **12.4** | [cite_start]On average, predictions are within ~12 units of actual sales[cite: 134]. |

> [cite_start]**Business Impact:** With high revenue accuracy[cite: 140], finance teams can rely on this model for inventory forecasting and margin planning.

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
