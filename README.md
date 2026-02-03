# CMAPSS Engine Remaining Useful Life (RUL) Prediction

## Project Overview

This project focuses on predicting the **Remaining Useful Life (RUL)** of aircraft engines using the CMAPSS dataset (FD001 subset). The aim is to develop machine learning models that can estimate when an engine will fail, allowing for predictive maintenance and minimizing downtime.

The workflow includes:

1. **Data Loading & Cleaning**
2. **Exploratory Data Analysis (EDA)**
3. **RUL Label Creation**
4. **Feature Scaling**
5. **Train–Validation Split**
6. **Model Training & Evaluation**
7. **Health State Classification**
8. **Visualization**

---

## Dataset

- **train_FD001.txt:** Engine training data (unit_number, time_in_cycles, operational settings, sensor readings)
- **test_FD001.txt:** Engine testing data
- **RUL_FD001.txt:** True Remaining Useful Life for test engines

**Features:**

- Operational Settings: `operational_setting_1`, `operational_setting_2`, `operational_setting_3`
- Sensor Measurements: `sensor_1` to `sensor_21` (after removing constant sensors)

---

## Data Preprocessing

- Removed null columns
- Removed constant sensor columns
- Created RUL labels capped at 125 cycles
- Scaled features using **MinMaxScaler** to avoid data leakage
- Split dataset into training and validation sets based on **engine units**

---

## Models Trained

- **RandomForestRegressor**
- **XGBoostRegressor**
- **LightGBMRegressor**

### Performance Metrics

| Model         | RMSE      | MAE       | R2 Score |
|---------------|-----------|-----------|----------|
| RandomForest  | 16.987793 | 12.300884 | 0.834165 |
| LightGBM      | 17.057722 | 12.210097 | 0.832797 |
| XGBoost       | 17.068138 | 12.258953 | 0.832593 |

> **Observation:** RandomForest achieved slightly better RMSE, while LightGBM shows consistent MAE performance.

---

## Health State Classification

To provide actionable insights, predicted RUL was converted into **health states**:

| Life Ratio Range | Health Class |
|-----------------|--------------|
| > 0.6           | Good (0)     |
| 0.3 – 0.6       | Moderate (1) |
| ≤ 0.3           | Warning (2)  |

**Classification Report (Validation Set)**

| Class     | Precision | Recall | F1-score | Support |
|-----------|-----------|--------|----------|---------|
| Good      | 0.88      | 0.96   | 0.92     | 2550    |
| Moderate  | 0.67      | 0.51   | 0.58     | 760     |
| Warning   | 0.92      | 0.86   | 0.89     | 760     |
| **Accuracy** | 0.86 | - | - | 4070 |

**Confusion Matrix:**  
- Shows clear separation between Good and Warning engines, Moderate engines have slightly lower recall.

---

## Visualizations

- **Sensor Trends:** Line plots for individual engine sensor readings
- **RUL Distribution:** Histograms to understand the distribution of Remaining Useful Life
- **Feature Importance:** XGBoost feature importance highlighting key sensors and operational settings
- **Correlation Heatmap:** Shows relationships between sensors and RUL
- **Actual vs Predicted RUL:** Scatter plots and residuals for model evaluation
- **Engine-level RUL Trends:** Overlay of predicted and actual RUL for sample engines

---

## Insights

- Certain sensors are more predictive of engine failure and heavily influence RUL predictions.
- The models perform well on “Good” and “Warning” classes but show moderate performance for “Moderate” engines.
- Predictive maintenance can be effectively planned using these models.

---

## Future Work

- Explore **LSTM/GRU models** for time-series RUL prediction.
- Hyperparameter tuning using **Bayesian Optimization**.
- Integrate additional CMAPSS datasets (FD002, FD003, FD004) for multi-condition engines.
- Deploy model for real-time engine monitoring with a dashboard.

---

## References

- [CMAPSS Dataset - NASA](https://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/)
- Scikit-learn, XGBoost, LightGBM Documentation

---

**Author:** Japneet Singh   
