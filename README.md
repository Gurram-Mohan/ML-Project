# 🌍 Air Quality Index Prediction Using XGBoost

## 📌 Overview

This project presents a Machine Learning approach for predicting air-quality values using historical pollution data collected by the **Central Pollution Control Board (CPCB), India**.

The project uses an **XGBoost Regressor** along with feature engineering, **Mutual Information (MI)** for feature relevance analysis, and **Variance Inflation Factor (VIF)** pruning to reduce multicollinearity among predictors.

The dataset contains **29,531 daily observations from 26 Indian cities**, covering the period from **January 2015 to July 2020**.

---

## 🎯 Objective

The main objective of this project is to develop a regression model capable of learning relationships between multiple pollutant measurements and air-quality values.

The project focuses on:

* Handling missing pollutant values
* Creating meaningful cross-pollutant features
* Selecting relevant features using Mutual Information
* Removing highly correlated predictors using VIF
* Comparing multiple regression models
* Training an XGBoost regression model
* Evaluating model performance using R², RMSE, MAE, and MSE

---

## 📊 Dataset

The dataset was obtained from the **Central Pollution Control Board (CPCB)**.

### Dataset Details

| Property           | Value                             |
| ------------------ | --------------------------------- |
| Total Records      | 29,531                            |
| Number of Cities   | 26                                |
| Date Range         | Jan 2015 – Jul 2020               |
| Pollutant Features | 12                                |
| AQI Range          | 13 – 2,049                        |
| Missing Values     | 7.0% – 61.3% depending on feature |

### Pollutants

The dataset contains measurements for:

* PM2.5
* PM10
* NO
* NO2
* NOx
* NH3
* CO
* SO2
* O3
* Benzene
* Toluene
* Xylene

The dataset also contains city, date, AQI, and AQI category information.

---

## ⚙️ Methodology

The overall workflow followed in this project is:

```text
Raw CPCB Dataset
       ↓
Data Cleaning
       ↓
Remove Unnecessary Columns
       ↓
Remove Duplicates
       ↓
Handle Missing Values
       ↓
Feature Engineering
       ↓
Mutual Information Ranking
       ↓
VIF-Based Feature Pruning
       ↓
Train/Test Split
       ↓
Model Training
       ↓
Model Evaluation
```

---

## 🧹 Data Preprocessing

Several metadata columns were removed because they were not directly required for the regression model.

Duplicate records were removed, and missing numerical values were handled using **median imputation**.

Median imputation was selected because many pollutant distributions are right-skewed and the median provides a more representative central value than the mean.

---

## 🛠️ Feature Engineering

Five cross-pollutant aggregate features were created:

### 1. `pollutant_max`

Represents the highest pollutant concentration among the selected core pollutants.

### 2. `pollutant_min`

Represents the lowest pollutant concentration.

### 3. `pollutant_range`

Calculated as:

```text
pollutant_max - pollutant_min
```

This represents the spread between the highest and lowest pollutant measurements.

### 4. `pollutant_ratio`

Calculated as:

```text
pollutant_max / (pollutant_min + 1)
```

The addition of `1` prevents division by zero.

### 5. `pollutant_avg`

Represents the arithmetic average of the nine core pollutant measurements.

These engineered features were used to capture relationships across multiple pollutants rather than relying only on individual pollutant values.

---

## 🔎 Feature Selection

### Mutual Information

Mutual Information was used to measure the statistical dependency between the available features and the prediction target.

The highest MI scores included:

| Feature         | MI Score |
| --------------- | -------: |
| pollutant_range |   1.1333 |
| pollutant_max   |   0.9678 |
| PM10            |   0.7870 |
| PM2.5           |   0.7740 |
| AQI             |   0.7377 |
| pollutant_ratio |   0.6196 |

The results showed that engineered cross-pollutant features contained substantial predictive information.

### VIF Pruning

After Mutual Information ranking, **Variance Inflation Factor (VIF)** was used to identify multicollinearity.

Features with VIF values above **10** were iteratively removed until the remaining predictors had VIF values below the threshold.

This helped reduce redundant information among highly correlated features.

---

## 🤖 Models Evaluated

The project considered five regression models:

1. Linear Regression
2. Random Forest
3. Gradient Boosting
4. Multi-Layer Perceptron (MLP)
5. XGBoost

### Model Comparison

| Model             |         R² |      RMSE |      MAE |
| ----------------- | ---------: | --------: | -------: |
| Linear Regression |     0.6814 |     11.23 |     8.76 |
| Random Forest     |     0.8391 |      6.38 |     4.89 |
| Gradient Boosting |     0.8449 |      6.21 |     4.67 |
| MLP               |     0.7923 |      7.26 |     5.53 |
| **XGBoost**       | **0.8625** | **5.637** | **4.21** |

The models were evaluated using the same processed feature set and the same 80/20 train-test split.

---

## 🚀 XGBoost Configuration

The final XGBoost model used:

```text
n_estimators = 400
learning_rate = 0.05
max_depth = 7
random_state = 42
```

The dataset was divided into:

```text
Training Samples: 23,624
Testing Samples:   5,907
```

---

## 📈 Results

The final XGBoost model achieved:

| Metric   |      Score |
| -------- | ---------: |
| **R²**   | **0.8625** |
| **RMSE** |  **5.637** |
| **MAE**  |   **4.21** |
| MSE      |     31.775 |

The model explained approximately **86% of the variance in the prediction target** on the held-out test set.

---

## ⭐ Feature Importance

The gain-based feature importance from XGBoost identified the following dominant predictors:

* **AQI**
* **pollutant_ratio**
* **pollutant_min**

Together, these features accounted for more than 99% of the reported gain-based importance.

The results also demonstrated the usefulness of engineered cross-pollutant features in the prediction pipeline.

---

## 🧰 Technologies Used

* **Python**
* **Pandas**
* **NumPy**
* **Scikit-learn**
* **XGBoost**
* **Matplotlib**
* **Seaborn**
* **Jupyter Notebook**
* **CPCB Dataset**

---

## 📁 Project Structure

```text
Air-Quality-Index-Prediction/
│
├── data/
│   └── air_quality.csv
│
├── notebooks/
│   └── AQI_Prediction.ipynb
│
├── README.md
│
└── requirements.txt
```

> Update the file and folder names above if your actual GitHub repository uses different names.

---

## ▶️ How to Run

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd Air-Quality-Index-Prediction
```

### 2. Install dependencies

```bash
pip install pandas numpy scikit-learn xgboost matplotlib seaborn jupyter
```

### 3. Start Jupyter Notebook

```bash
jupyter notebook
```

Open the project notebook and run the cells sequentially.

---

## ⚠️ Limitations

The project has several limitations:

* The train-test split was random rather than temporal.
* High-missingness features such as Xylene were handled using median imputation.
* Meteorological variables such as temperature, humidity, wind speed, and atmospheric pressure were not included.
* No cross-validation or extensive hyperparameter search was performed.
* The model showed some underestimation for higher values of the target variable.

These limitations are important when interpreting the reported test performance.

---

## 🔮 Future Improvements

Possible improvements include:

* Using a **temporal train-test split** for more realistic evaluation.
* Applying more advanced missing-value techniques.
* Adding meteorological features such as temperature, humidity, and wind speed.
* Using missingness indicator variables.
* Performing cross-validation.
* Applying hyperparameter optimization.
* Comparing against sequence-based models such as LSTM or Transformer architectures.

---

## 👨‍💻 Authors

**Mohan Gurram**
Computer Science and Engineering

**Mahidhar Bodduluri**
Computer Science and Engineering

**Nikhil Sumanth Kiretty**
Computer Science and Engineering

---

## 📜 Project Summary

This project demonstrates how machine learning can be applied to air-quality data by combining **feature engineering, Mutual Information, VIF-based feature pruning, and XGBoost regression**.

The final model achieved an **R² of 0.8625**, with an **RMSE of 5.637** and **MAE of 4.21** on 5,907 held-out samples.
