# Bank Term Deposit Subscription — Data Exploration & Preprocessing

## Project Overview
This repository contains the exploratory data analysis (EDA) and data preprocessing pipeline for a bank marketing dataset (`bank-full.xlsx`). The notebook prepares demographic, financial, and campaign contact records of 45,211 bank clients for downstream predictive modeling to determine whether a client will subscribe to a term deposit (`y`).

## Problem Statement
Financial institutions conduct direct marketing campaigns (primarily via phone calls) to promote term deposits. Contacting every customer indiscriminately leads to high operational costs and low conversion rates. The business problem is to clean, engineer features from, and structure historical campaign and customer profile data so machine learning models can accurately identify clients most likely to subscribe (`yes` vs. `no`).

## Objectives
* Load and inspect the raw banking dataset (`45,211` records across `17` initial features).
* Remove post-hoc or non-generalizable features (`month`, `day`, `duration`) to prevent data leakage and focus on client attributes.
* Engineer a domain-specific `income_group` feature by mapping client job categories into `High`, `Mid`, and `Low` tiers.
* Handle missing values and verify data integrity across all numerical and categorical variables.
* Perform Exploratory Data Analysis (EDA) to examine class imbalance and statistical distributions of numerical features.
* Encode binary and multi-class categorical variables into a model-ready numerical matrix.

## Technologies Used
* **Language:** Python 3
* **Environment:** Google Colab / Jupyter Notebook
* **Libraries:**
  * `pandas` — Data loading, manipulation, missing-value handling, and one-hot encoding
  * `numpy` — Numerical operations
  * `matplotlib` & `seaborn` — Statistical visualization and distribution plotting (`histplot` with KDE)
  * `scikit-learn` — Imported preprocessing utilities (`LabelEncoder`, `StandardScaler`)

## Methodology

### 1. Data Loading & Feature Selection
* Loaded `bank-full.xlsx` containing **45,211 rows** and **17 columns** (`age`, `job`, `marital`, `education`, `default`, `balance`, `housing`, `loan`, `contact`, `day`, `month`, `duration`, `campaign`, `pdays`, `previous`, `poutcome`, `y`).
* Dropped `month`, `day`, and `duration`, reducing the dataset shape to `(45211, 14)`.

### 2. Feature Engineering & Data Cleaning
* Created `income_group` by mapping the `job` column:
  * **High:** `management`, `entrepreneur`, `self-employed`
  * **Mid:** `technician`, `admin.`, `services`, `retired`
  * **Low:** `blue-collar`, `housemaid`, `student`, `unknown`
* Dropped the original `job` column.
* Identified **1,303** unmapped/null entries in `income_group` (corresponding to job categories not present in the mapping dictionary, such as `unemployed`) and removed them using `dropna()`, resulting in **43,908 clean records** with **0 missing values** across all 14 columns.

### 3. Exploratory Data Analysis (EDA)
* Analyzed target variable (`y`) distribution before and after cleaning.
* Separated features into **8 categorical columns** (`marital`, `education`, `default`, `housing`, `loan`, `contact`, `poutcome`, `income_group`) and **5 numerical columns** (`age`, `balance`, `campaign`, `pdays`, `previous`).
* Generated summary statistics (`describe()`) and plotted 30-bin histograms with Kernel Density Estimates (KDE) for all numerical features.

### 4. Categorical Encoding
* Mapped binary columns (`default`, `housing`, `loan`, `y`) from `'yes'`/`'no'` to `1`/`0`.
* Applied One-Hot Encoding (`pd.get_dummies(..., drop_first=True, dtype=int)`) to `income_group`, `marital`, `education`, `contact`, and `poutcome`, producing a final preprocessed dataset (`df_encoded`) of **43,908 rows × 21 features**.

## Models Implemented
* *Note:* The current notebook covers **Data Exploration and Preprocessing** only. No machine learning or Artificial Neural Network (ANN) models have been trained in this notebook yet.

## Model Evaluation
* Not applicable in the current preprocessing stage. The output of this pipeline (`df_encoded`) serves as the clean feature matrix for model training and evaluation in subsequent stages.

## Results & Insights
* **Class Imbalance:**
  * Raw dataset (`45,211` rows): `39,922` **no** vs. `5,289` **yes**.
  * Cleaned dataset (`43,908` rows): `38,821` **no** (**88.41%**) vs. `5,087` **yes** (**11.59%**), confirming a significant class imbalance that will require stratified splitting, class weights, or resampling during modeling.
* **Numerical Feature Summary (`n = 43,908`):**

| Feature | Mean | Std Dev | Min | 25% | Median (50%) | 75% | Max |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| `age` | 40.94 | 10.64 | 18 | 33 | 39 | 48 | 95 |
| `balance` | 1,357.54 | 3,041.66 | -8,019 | 71 | 446 | 1,423 | 102,127 |
| `campaign` | 2.77 | 3.11 | 1 | 1 | 2 | 3 | 63 |
| `pdays` | 40.38 | 100.29 | -1 | -1 | -1 | -1 | 854 |
| `previous` | 0.58 | 2.32 | 0 | 0 | 0 | 0 | 275 |

* **Key Data Observations:**
  * Over 75% of clients have `pdays = -1` and `previous = 0`, indicating they were not contacted in a previous campaign.
  * Account `balance` is heavily right-skewed (median `446` vs. mean `1,357.54`, max `102,127`), with negative balances going down to `-8,019`.
  * Final encoded feature set contains 21 integer/numeric columns ready for scaling and classification.

## Project Structure
```text
├── data/
│   └── bank-full.xlsx          # Raw input dataset (45,211 records)
├── notebooks/
│   └── preprocessing_eda.ipynb # Jupyter/Colab notebook with cleaning, EDA, and encoding
└── README.md                   # Project documentation

How to Run
1. Clone the Repository
git clone [https://github.com/](https://github.com/)<your-username>/<your-repo-name>.git
cd <your-repo-name>
2. Install Dependencies
pip install pandas numpy matplotlib seaborn scikit-learn openpyxl jupyter
3. Update Dataset Path & Run Notebook
     1. Place bank-full.xlsx in your working directory (or update the file path in cell 2 from "/content/bank-full.xlsx" to your local path, e.g., "data/bank              full.xlsx").
     2. Launch Jupyter Notebook or open the .ipynb file in Google Colab:
        jupyter notebook

















