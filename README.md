# 🏦 Home Loan Default Prediction

## 📌 Project Overview

This project focuses on predicting the likelihood of home loan default using machine learning techniques.

The project was developed as part of the DataMites internship project **PRCP-1006-HomeLoanDefault**.

The objective is to analyze customer application and historical credit-related information, identify important risk factors, build predictive classification models, and segment customers based on their predicted default risk.

---

## 🎯 Problem Statement

The project has two major objectives:

1. Perform a complete data analysis of the available home loan application and historical credit data.
2. Build a predictive machine learning model to identify customers who may have a higher risk of loan default.

---

## 💼 Business Objective

Financial institutions need to assess credit risk before approving loans.

A predictive model can help identify customers who show higher observed default risk based on factors such as:

- External credit indicators
- Employment stability
- Credit-to-income relationship
- Credit-to-goods relationship
- Previous credit behaviour
- Previous loan applications
- Payment history

The model can support risk-based application review while keeping human decision-making in the loop.

---

## 📊 Dataset

The project uses the Home Credit loan application dataset.

The analysis integrates information from multiple related tables:

- `application_train.csv`
- `bureau.csv`
- `bureau_balance.csv`
- `previous_application.csv`
- `POS_CASH_balance.csv`
- `credit_card_balance.csv`
- `installments_payments.csv`

The main target variable is:

- `TARGET = 0` → Non-default
- `TARGET = 1` → Default

### Target Distribution

The working dataset contains:

- Total customers: **1,000**
- Non-default customers: **930**
- Default customers: **70**
- Default rate: **7%**

---

## 🔗 Data Integration

Customer-level features were created from the historical tables and merged with the main application data.

The project creates aggregated features representing:

- Previous credit history
- Previous loan applications
- POS/cash loan behaviour
- Credit card behaviour
- Installment payment behaviour
- Delinquency and late-payment behaviour

The final modeling dataset contains **160 features before categorical encoding**.

After preprocessing, categorical variables were one-hot encoded, resulting in **275 processed features**.

---

## 🧹 Data Preprocessing

The following preprocessing techniques were applied:

- Missing-value handling
- Duplicate checking
- Constant-feature removal
- Categorical feature handling
- Numerical feature scaling
- One-hot encoding
- Anomaly handling for `DAYS_EMPLOYED`
- Feature engineering
- Stratified train-test splitting

A major anomaly was identified in `DAYS_EMPLOYED`, where a very large positive value represented an abnormal employment-duration value. This was handled before modeling.

---

## 🛠️ Feature Engineering

Several meaningful features were created, including:

### Age

Converted `DAYS_BIRTH` into:

`AGE_YEARS`

### Employment Experience

Converted `DAYS_EMPLOYED` into:

`EMPLOYMENT_YEARS`

### Credit-to-Income Ratio

Measures the relationship between requested credit and customer income.

### Annuity-to-Income Ratio

Measures the loan annuity relative to customer income.

### Credit-to-Goods Ratio

Compares requested credit with the value of the associated goods.

### Income per Family Member

Measures income relative to the number of family members.

Historical customer-level features were also created from the Bureau, Previous Application, POS Cash, Credit Card, and Installment Payment datasets.

---

## 🤖 Machine Learning Models

Four classification algorithms were evaluated:

1. Logistic Regression
2. Decision Tree
3. Random Forest
4. Gradient Boosting

Because the dataset contains only about 7% default cases, class imbalance was considered during model development.

---

## 📈 Model Evaluation

The models were evaluated using:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
- PR-AUC
- Stratified Cross-Validation

ROC-AUC was used to evaluate ranking ability, while PR-AUC was particularly useful because of the imbalanced target distribution.

---

## 🔄 Cross-Validation Results

5-fold Stratified Cross-Validation produced the following ROC-AUC results:

| Model | Mean CV ROC-AUC |
|---|---:|
| Logistic Regression | 0.6242 |
| Decision Tree | 0.6402 |
| Random Forest | 0.6732 |
| Gradient Boosting | **0.7348** |

Gradient Boosting achieved the highest mean cross-validation ROC-AUC among the tested models.

---

## 🏆 Final Model

The final selected model was:

**Gradient Boosting Classifier**

The model was evaluated on an untouched test set.

### Final Test Performance

- Accuracy: **79.50%**
- Precision: **17.07%**
- Recall: **50.00%**
- F1-score: **25.45%**
- ROC-AUC: **0.6576**
- PR-AUC: **0.1453**

An out-of-fold threshold-selection approach produced a final classification threshold of **0.08**.

This threshold was selected using training out-of-fold predictions and then evaluated on the untouched test set.

---

## ⚖️ Handling Class Imbalance

Only approximately 7% of customers in the working dataset were defaulters.

Therefore, accuracy alone would not be sufficient for evaluating the model.

The project addressed imbalance using:

- Stratified train-test splitting
- Class weighting where applicable
- ROC-AUC
- PR-AUC
- Recall
- F1-score
- Out-of-fold threshold optimization

The lower classification threshold was used to improve the model's ability to identify potential default cases.

---

## 🔍 SHAP Explainability

SHAP (SHapley Additive exPlanations) was used to understand model predictions.

The major predictive features identified through SHAP included:

1. `EXT_SOURCE_2`
2. `EXT_SOURCE_3`
3. `EMPLOYMENT_YEARS`
4. `CREDIT_GOODS_RATIO`
5. `HOUR_APPR_PROCESS_START`
6. `CREDIT_INCOME_RATIO`
7. `DAYS_LAST_PHONE_CHANGE`
8. `EXT_SOURCE_1`

The analysis showed that lower external-source scores and shorter employment history were associated with higher predicted default risk in the sample.

SHAP explanations describe model behaviour and should not be interpreted as proof of causation.

---

## 🚦 Risk Segmentation

Customers were divided into analytical risk categories using predicted default probabilities:

| Probability | Risk Category |
|---|---|
| `< 0.10` | Low Risk |
| `0.10 – < 0.30` | Medium Risk |
| `0.30 – < 0.50` | High Risk |
| `>= 0.50` | Very High Risk |

These are analytical project-level categories and are **not official banking approval thresholds**.

---

## 📊 Probability Calibration

Probability calibration was also evaluated using a calibration curve and Brier Score.

The test-set Brier Score was:

**0.0708**

The calibration analysis showed a broadly reasonable relationship between predicted probabilities and observed outcomes, although the small number of default cases makes the result unstable.

---

## ⚠️ Challenges & Limitations

The project has several important limitations:

- Strong class imbalance
- Small working dataset
- Only 14 default cases in the test set
- Historical tables were sampled during development
- Potential temporal leakage because a strict historical cutoff was not enforced
- High-dimensional processed feature space
- Small high-risk customer groups
- Probability calibration is limited by sample size

Therefore, the model should be considered a **proof-of-concept analytical model**, not a production-ready banking decision system.

---

## 💡 Business Recommendations

Potential business applications include:

- Risk-based application review
- Additional verification for medium/high-risk applicants
- Detailed assessment of high-risk applications
- Use of historical repayment behaviour during risk assessment
- Explainable predictions using SHAP
- Human-in-the-loop decision making

The selected threshold should ultimately be determined using business costs, regulatory requirements, and validation on a much larger dataset.

---

## 🔮 Future Improvements

Future versions could include:

- Using the complete historical datasets
- Strict time-aware feature engineering
- Preventing temporal leakage
- Pipeline-based imputation and preprocessing
- Hyperparameter optimization
- Better probability calibration
- Cost-sensitive threshold selection
- Independent external validation
- Model monitoring and drift detection
- Larger and more representative datasets

---

## 🧰 Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- SHAP
- Google Colab
- Jupyter Notebook

---

## 📁 Project Structure

```text
Home-Loan-Default-Prediction/
│
├── PRCP_1006_HomeLoanDefault_v3.ipynb
├── README.md
├── requirements.txt
│
└── images/
    └── project_visualizations/

---

## 👨‍💻 Author

**Harsh Bandekar**

BSc Computer Science | Data Science

Areas of interest:

- Data Science
- Machine Learning
- Data Analytics
- Python
- SQL
- Data Visualization

---

## ⭐ Project Note

This project was developed for educational and internship purposes to demonstrate an end-to-end machine learning workflow for credit-risk analysis.

The model outputs should not be treated as financial or lending advice.