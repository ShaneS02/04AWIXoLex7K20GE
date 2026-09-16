# Term Deposit Subscription Prediction

# Goals

- Predict if the customer will subscribe to a term deposit
- Determine the segment(s) of customers who are more likely to buy the investment product.
- Determine most important feature that makes the customers buy

## Dataset

The target variable

Y => has the client subscribed to a term deposit?

includes:

- `yes` → customer subscribed
- `no` → customer did not subscribe

### Features

The dataset contains:

# Features

| Name        | Type        | Description                                                                   |
| ----------- | ----------- | ----------------------------------------------------------------------------- |
| `age`       | Numeric     | Age of the customer                                                           |
| `job`       | Categorical | Type of job                                                                   |
| `marital`   | Categorical | Marital status                                                                |
| `education` | Categorical | Level of education                                                            |
| `default`   | Binary      | Whether the customer has credit in default                                    |
| `balance`   | Numeric     | Average yearly balance, in euros                                              |
| `housing`   | Binary      | Whether the customer has a housing loan                                       |
| `loan`      | Binary      | Whether the customer has a personal loan                                      |
| `contact`   | Categorical | Type of contact communication                                                 |
| `day`       | Numeric     | Last contact day of the month                                                 |
| `month`     | Categorical | Last contact month of the year                                                |
| `duration`  | Numeric     | Duration of the last contact, in seconds                                      |
| `campaign`  | Numeric     | Number of contacts performed during this campaign, including the last contact |

`duration` was removed because it represents the length of the phone call and is only known after the interaction. Using it for targeting would introduce information that would not be available when deciding which customers to contact.

---

## Methodology

### Preprocessing

Different preprocessing methods were used based on feature type:

- Numerical features → `StandardScaler`
- Categorical features → `One-Hot Encoding`
- Binary categorical features → `Ordinal Encoding`

Preprocessing and feature selection were included inside the model pipelines to prevent information leakage during cross-validation.

### Feature Selection

Mutual Information was used to measure the relationship between features and the target.

The models tested different numbers of selected features:

```text
3, 5, 7, 10, or all
```

The best configuration was selected using cross-validated **F1 score**.

### Models

The following models were compared:

- Logistic Regression
- XGBoost
- KNN

SVM was considered but removed because it required substantially more training time without providing a significant performance improvement.

### Cross-Validation

Models were evaluated using **5-fold Stratified Cross-Validation**.

Stratification maintains a similar proportion of subscribers and non-subscribers across the folds.

---

# Evaluation Metrics

## Why Accuracy Is Not Reliable

The target variable is highly imbalanced, with substantially more customers who **do not subscribe** than customers who do.

Because of this, a model can achieve high accuracy by simply predicting customers do not subscribe.

Therefore **F1 score was used to select the best model**.

### F1 Score

F1 balances:

- **Precision** — how many predicted buyers actually subscribe.
- **Recall** — how many actual buyers the model successfully identifies.

This makes F1 more appropriate for comparing models for the customer-targeting objective.

---

# Results

### 5-Fold Cross-Validation

| Model               |   Accuracy | Precision | Recall |         F1 |
| ------------------- | ---------: | --------: | -----: | ---------: |
| Logistic Regression |     69.95% |    12.68% | 53.52% |     20.51% |
| **XGBoost**         | **84.67%** |    19.09% | 34.57% | **24.59%** |
| KNN                 | **92.62%** |    46.06% | 10.75% |     17.40% |

### Key Observation

KNN achieved the highest accuracy at **92.62%**, but its F1 score was only **17.40%** and recall was **10.75%**.

This demonstrates why accuracy alone is misleading. KNN is very effective at predicting the majority class but misses most actual subscribers.

XGBoost achieved a lower accuracy of **84.67%**, but had the highest F1 score at **24.59%**, making it the better model for the actual business objective.

---

# Best Model: XGBoost

XGBoost was selected as the preferred model because it achieved the highest F1 score among the tested models.

### XGBoost Cross-Validation

- Accuracy: **84.67%**
- Precision: **19.09%**
- Recall: **34.57%**
- F1: **24.59%**

### XGBoost Test Set

- Accuracy: **84.55%**
- Precision: **20.43%**
- Recall: **39.21%**
- F1: **26.86%**

The similar cross-validation and test results suggest that the model's performance is reasonably consistent on unseen data.

### Points of interest

Among all the models used, the analysis identified the following features as particularly useful in the feature-selection results:

- `contact`
- `month`
- `age`
- `balance`
- `day`

XGBoost ultimately performed best using **all available features**, indicating that additional variables can provide useful information when XGBoost models nonlinear relationships and feature interactions.

---

# What Makes Customers Buy?

The analysis suggests that **contact method and campaign timing**, along with customer demographic and financial characteristics, are important predictive signals.

The most consistently selected features were:

### 1. `contact`

The method used to contact the customer provides useful predictive information.

### 2. `month`

The timing of the campaign is also an important predictor.

### 3. `age`

Customer age provides useful information for distinguishing potential subscribers.

### 4. `balance`

Account balance provides additional predictive information.

### 5. `day`

The timing of the campaign at the day level also contributes predictive information.

# Customer Segments to Prioritize

The XGBoost model was used to predict each customer’s likelihood of subscribing and ranked them from highest to lowest probability. The highest-priority customers were more commonly reached by cellular contact and were disproportionately represented in April campaigns. They were also more likely to have tertiary education, be single, and have no housing or personal loan. Management and technician roles were also more common among this group.

Therefore, the bank should prioritize customers matching this high-probability profile, using the XGBoost subscription probability to determine the order in which customers should be targeted.

---

# Final Conclusions

- Accuracy of the XGBoost model was 84.67%
- **XGBoost had the highest F1 score (24.59%)** and is therefore the preferred model.
- The dataset's class imbalance makes accuracy an unreliable measure of the model's ability to identify potential buyers.
- `contact`, `month`, `age`, `balance`, and `day` were consistently useful features during feature selection.
- High-priority customers were more likely to have cellular contact, tertiary education, be single, and have no housing or personal loan.
