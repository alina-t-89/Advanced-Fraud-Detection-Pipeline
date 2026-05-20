# 🛡️ Advanced Fraud Detection System & Behavior Feature Engineering

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data_Analysis-orange.svg)](https://pandas.pydata.org/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-Machine_Learning-green.svg)](https://scikit-learn.org/)
[![Status](https://img.shields.io/badge/Project_Status-Completed-brightgreen.svg)]()

## 📌 Project Overview
This repository contains a production-grade machine learning pipeline for exploring, optimizing, and engineering behavioral risk features using the high-scale **IEEE-CIS Fraud Detection dataset (Vesta Corporation)**. 

The core challenge of this project is managing **extreme class imbalance (~3.5% fraud rate)** and dealing with heavily anonymized, fragmented data spanning over 590,000 transaction records. Moving beyond standard metrics, this project implements a dynamic business-threshold framework to balance strict fraud mitigation with user experience.

---

## 🛠️ Implemented Pipeline Architecture

### 1. Memory Management & Data Downcasting
* Standardized a custom downcasting algorithm (`reduce_mem_usage`) to safely handle large-scale data files.
* Optimized system RAM consumption, ensuring smooth dataframe merging and manipulation on standard development environments.

### 2. Multi-Source Identity Merging
* Executed a strategic `LEFT JOIN` on multi-source datasets utilizing `TransactionID` as the primary key.
* Extracted cross-device indicators, focusing on device environments (`DeviceType`), web clients (`id_31`), and proxy verification frameworks (`id_15`).

### 3. Advanced Behavioral Feature Engineering
To give the model predictive "eyes," the pipeline shifts from individual transaction records to historical cardholder context, implementing specific data-safety shields:
* **Temporal Risk Signals:** Extracted a cyclical `HourOfDay` indicator from raw transaction deltas (`TransactionDT`) to capture high-velocity nocturnal fraud waves.
* **Composite Card Signatures:** Combined multi-variable card data into a unified `Card_ID` profile to track individual historical patterns.
* **Velocity & Anomaly Indicators:** Engineered `Time_Since_Last_Trans` and `Is_Rapid_Fire` to detect automated bot attacks (multiple transactions within seconds).
* **Contextual Normalization:** Generated `Amt_to_Mean_Card` and `Amt_to_Std_Card` metrics via `.transform()` window functions, isolating transactions that deviate sharply from the user's historical baseline.
* **Zero-Division Shield:** Implemented a robust data-scaling fix replacing empty or zero standard deviations (`fillna(1)` and `replace(0, 1)`) to guarantee mathematical stability and prevent infinite values during inference.

---

## 📈 Iterative Model Evolution & The "Fail-Fast" Journey

Building a production-grade anti-fraud system is never a straight line. Below is the historical breakdown of how the model evolved from a naive baseline to a highly balanced, risk-aware ensemble.

### 🚫 Step 1: The Naive Baseline (Decision Tree)
* **Setup:** `DecisionTreeClassifier(max_depth=5)` on raw baseline features.
* **The Result:** High overall accuracy (~96%), but completely blind to fraud. It caught almost zero fraud cases because it simply biased toward the majority class (legitimate transactions).
* **The Lesson:** In highly imbalanced datasets, standard accuracy is a vanity metric. We needed to force the model to care about the minority class.

### ⚠️ Step 2: The "Paranoid" Balanced Tree (Automatic Class Weights)
* **Setup:** Introduced `class_weight='balanced'` to heavily penalize missing fraud.
* **The Result:** The model went into a state of panic. While it caught more fraud, it triggered over **37,000 False Positives** (falsely blocking legitimate users). 
* **The Lesson:** Blindly using automated balancing on a weak model completely destroys user experience (UX) and paralyzes business operations.

### 🔍 Step 3: The Volatility Trap & The Sorting Discovery
* **Setup:** Adjusted class weights manually to `{0: 1, 1: 7}` to tone down the paranoia.
* **The Sandbox Trap:** In the initial chronological sandbox (sorted strictly by timestamp), this setup caught **121** fraud cases with **1,244** False Positives.
* **The Reality Check:** Once the dataset was correctly grouped by `Card_ID` to prepare for advanced behavioral features, the exact same baseline tree completely degraded, catching only **22** fraud cases and dropping False Positives to 177.
* **The Critical Insight:** This collapse proved that the single Decision Tree wasn't learning real fraud patterns—it was just picking up volatile temporal noise. If data order changes, a simple tree blind to context completely breaks. This structural failure fully justified the transition to **Robust Ensembles (Random Forest)** and deep **Feature Engineering**.

---

## 🏆 Final Production Results & Strategy Simulation

By upgrading to a deeper ensemble (`max_depth=12`) and leveraging our 6 contextual features, we unlocked the ability to extract smooth probability scores (`predict_proba`) and gave the business a flexible steering wheel. 

Instead of relying on rigid default boundaries, we simulated 3 distinct operational strategies based on the **Anti-Fraud Precision Ratio (AFPR)**:

| Strategy Mode | Threshold | True Positives (Fraud Caught) | False Positives (Legit Blocked) | Operational AFPR (False Alarms per 1 Fraud) | Business Context |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Aggressive Intercept** | `65%` | **1,542** | 6,842 | 4.44 | Used during active cyber-attacks or high-risk periods. Prioritizes maximum security. |
| **Balanced Growth** | `75%` | **615** | 1,835 | **2.98** | **Optimal Production Mode.** Drastically cuts false alarms while maintaining a strong, steady grip on fraud. |
| **VIP / High-Loyalty** | `85%` | **22** | 25 | **1.14** | Laser-focused precision. Minimal user friction, ideal for premium accounts or massive transaction amounts. |

**Core Takeaway:** The project successfully demonstrated that moving from rigid, volatile baselines to contextual features combined with custom operational thresholds allows a financial institution to dynamically balance risk mitigation with customer satisfaction.

---

## 🚀 How to Run Locally

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/YOUR_USERNAME/YOUR_REPOSITORY_NAME.git](https://github.com/YOUR_USERNAME/YOUR_REPOSITORY_NAME.git)
