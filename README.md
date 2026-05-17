# 🛡️ Advanced Fraud Detection System & Behavior Feature Engineering

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data_Analysis-orange.svg)](https://pandas.pydata.org/)
[![Status](https://img.shields.io/badge/Project_Status-In_Progress-yellow.svg)]()

## 📌 Project Overview
This repository contains a production-grade data pipeline for exploring, optimizing, and engineering behavioral risk features using the high-scale **IEEE-CIS Fraud Detection dataset (Vesta Corporation)**. 

The core challenge of this project is managing **extreme class imbalance (~3.5% fraud rate)** and dealing with heavily anonymized, fragmented data spanning over 500,000 transaction records.

> ⚠️ **Project Status: In Active Development** > *Current Phase: Advanced Feature Engineering & Behavioral Anomaly Validation. Next Phase: Model Training (LightGBM/XGBoost).*

---

## 🛠️ Implemented Pipeline Architecture

### 1. Memory Management & Data Downcasting
* Standardized a custom downcasting algorithm (`reduce_mem_usage`) to safely handle large-scale data files (`train_transaction.csv` & `train_identity.csv`).
* Optimized system RAM consumption, ensuring smooth dataframe merging and manipulation on standard development environments.

### 2. Multi-Source Identity Merging
* Executed a strategic `LEFT JOIN` on multi-source datasets utilizing `TransactionID` as the primary key.
* Extracted cross-device indicators, focusing on device environments (`DeviceType`), web clients (`id_31`), and proxy verification frameworks (`id_15`).

### 3. Advanced Behavioral Feature Engineering
* **Temporal Risk Signals:** Extracted a cyclical `HourOfDay` indicator from raw transaction deltas (`TransactionDT`) to capture high-velocity nocturnal fraud waves.
* **Composite Card Signatures:** Combined multi-variable card data (`card1`, `card2`, `card3`, `card5`) into a unified `Card_ID` profile to track individual historical patterns.
* **Velocity & Normalization Checks:** Engineered the `Amt_to_Mean_Card` metric via `.transform()` window functions, isolating immediate transactions that deviate sharply from the user's historical average.

---

## 📈 Key Analytical Insights

* **Severe Class Imbalance:** Verified that fraudulent transactions account for only **~3.5%** of the entire dataset, requiring specific cost-sensitive learning metrics going forward.
* **Device Vulnerability:** Found that **Mobile** devices exhibit a significantly higher fraud density compared to Desktop setups.
* **Feature Lift Validation:** The custom engineered feature `Amt_to_Mean_Card` successfully isolated an anomaly segment where the transaction value exceeded the historical mean by 5x. **Within this engineered segment, the fraud concentration increased significantly compared to the baseline dataset rate.**

---

## 🚀 How to Run Locally

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/YOUR_USERNAME/YOUR_REPOSITORY_NAME.git](https://github.com/YOUR_USERNAME/YOUR_REPOSITORY_NAME.git)
