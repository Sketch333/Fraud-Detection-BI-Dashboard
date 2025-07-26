# 💳 Credit Card Fraud Detection Dashboard (Power BI)

Fraud costs the finance industry billions each year. This project documents the full pipeline of building a **Power BI Dashboard** for detecting and analyzing fraudulent transactions using the [Credit Card Fraud Detection dataset on Kaggle](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud).

## 📌 Project Overview

This dashboard explores transaction patterns, highlights suspicious behavior, and makes the data explorable via slicers and interactive visuals. It covers every major step:

- Data Extraction  
- Data Transformation  
- Data Modeling  
- Visualization  
- Insight Generation  

---

## 📂 1. Data Extraction

### 📊 Source

- Dataset: [Credit Card Fraud Detection - Kaggle](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)  
- Time Period: 2 days in September 2013  
- Transactions: 284,807  
- Fraud Cases: 492  
- Features: 30  
  - V1–V28 (PCA-transformed), Amount, Time, Class

### 🔍 Method

- Imported manually via **Power BI → Get Data → CSV**
- For prototyping only — real-world use would involve databases or secure APIs

---

## 🛠️ 2. Data Transformation

### ✔️ Data Cleaning

- No missing values, duplicates, or format issues  
- Verified data types:
  - Numeric (float): V1–V28, Amount  
  - Integer: Class

### 🧪 Feature Engineering

#### a. Hour of Transaction
```DAX
Hour = INT('creditcard'[Time] / 3600)
Helps track fraud frequency across different times of day.

#### b. Amount Category
```DAX
AmountCategory = 
    SWITCH(TRUE(),
        creditcard[Amount] < 10, "Small",
        creditcard[Amount] < 100, "Medium",
        "Large"
    )
Segments transactions for value-based pattern analysis.

#### c. Class Label
```DAX
ClassLabel = IF(creditcard[Class] = 1, "Fraud", "Legit")
Improves visual clarity in charts.

### 📏 Key Measures (DAX)
```DAX

Total Transactions = COUNTROWS(creditcard)
Fraud Transactions = CALCULATE(COUNTROWS(creditcard), creditcard[Class] = 1)
Fraud Rate (%) = DIVIDE([Fraud Transactions], [Total Transactions])
Total Amount = SUM(creditcard[Amount])
Total Fraud Amount = CALCULATE(SUM(creditcard[Amount]), creditcard[Class] = 1)
Average Transaction Amount = AVERAGE(creditcard[Amount])
Average Fraud Amount = CALCULATE(AVERAGE(creditcard[Amount]), creditcard[Class] = 1)
## 🗂️ 3. Data Loading & Modeling
Import Mode used for performance
Flat table — no relationships or dimensional modeling needed
Future iterations can integrate temporal or geographic dimensions

##📈 4. Dashboard Layout
The dashboard is structured into main pages:

### 📊 KPI Summary & Overview
KPI Cards:
✅ Total Transactions: 284,807

⚠️ Fraud Transactions: 492

📉 Fraud Rate: ~0.17%

💰 Total Processed Amount: ~$25M

Pie Chart:
Fraud vs Legit transaction ratio

99.83% Legit vs 0.17% Fraud

Bar Chart:
Fraud by Amount Category

Most frauds occur in the Large category

### ⏱ Time & Detail Analysis
Line Chart:
Fraud Frequency by Hour

Spikes during unusual hours (late night/early morning)

Stacked Column:
Class by Hour

Legit transactions during business hours vs. fraud outside typical patterns

Table:
Top 10 Fraudulent Transactions

High-value cases for closer inspection

Slicers:
Filter by Hour or AmountCategory to explore specific behavior
