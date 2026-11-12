# Network Intrusion Detection System (NIDS) Using Machine Learning

## Project Overview

This project focuses on developing a **Network Intrusion Detection System (NIDS)** using Machine Learning techniques to identify malicious network traffic and distinguish between benign and attack flows.

The project follows the **CRISP-DM methodology** and uses the **NF-ToN-IoT-v3 NetFlow dataset**. It aims to maximize attack detection while minimizing false alarms, with an operational constraint of a False Positive Rate (FPR) below 1%.

The system performs both:
- **Binary Classification:** Distinguishing between benign and malicious network traffic.
- **Multiclass Classification:** Identifying the specific type of network attack.

## Dataset

- **Dataset:** NF-ToN-IoT-v3
- **Data type:** NetFlow network traffic records.
- **Original dataset size:** Approximately 27.5 million flows.
- **Working sample:** 3 million flows selected using stratified sampling.
- **Target variables:**
  - `Label`: Binary target (0 = Benign, 1 = Attack).
  - `Attack`: Attack category used for stratification and multiclass classification.

The sampling strategy preserves the original distribution of benign traffic and attack categories, including rare attack classes.

## Methodology – CRISP-DM

### 1. Business Understanding
Define the main objective: detect malicious network traffic while maximizing attack recall under an FPR constraint of 1%.

### 2. Data Understanding
- Analyze the dataset structure and statistical distributions.
- Examine attack categories and class imbalance.
- Investigate missing values, duplicates and feature relationships.
- Explore network traffic characteristics through visualizations.

### 3. Data Preparation
- Clean invalid labels and inconsistent records.
- Handle infinite values and missing data.
- Exclude identifiers, IP addresses, timestamps and potentially misleading variables.
- Remove redundant numerical features using correlation analysis.
- Select 43 predictive features.
- Apply categorical encoding and numerical preprocessing.
- Split the data into training (80%) and testing (20%) sets.

### 4. Modeling
Four machine learning algorithms are compared:

- XGBoost
- Logistic Regression (SGD)
- Decision Tree
- Linear SVM (SGD)

Hyperparameter tuning is performed using GridSearchCV. Models are evaluated using stratified 5-fold cross-validation.

The classification threshold is calibrated using out-of-fold (OOF) predictions to maximize weighted attack recall while respecting the FPR constraint.

### 5. Evaluation
The selected model is evaluated on an independent test set that is kept separate from model selection and threshold calibration.

Evaluation metrics include:
- Precision and Recall
- F1-score
- PR-AUC and ROC-AUC
- Matthews Correlation Coefficient (MCC)
- Balanced Accuracy
- False Positive Rate (FPR)
- Confusion Matrix

### 6. Deployment
Export the selected binary classification model, its preprocessing pipeline, the calibrated threshold, and the multiclass attack classifier using Joblib.

## Model Performance

### Binary Classification – XGBoost

XGBoost achieved the best overall results under the operational FPR constraint.

| Metric | Test Result |
|---|---:|
| PR-AUC | 0.9730 |
| ROC-AUC | 0.9824 |
| Attack Recall | 73.34% |
| Attack Precision | 98.33% |
| F1-score | 0.8402 |
| MCC | 0.7792 |
| Balanced Accuracy | 86.27% |
| False Positive Rate | 0.795% |

**Key result:** The selected XGBoost model achieved a 73.34% attack recall while keeping the test FPR below 1%.

### Multiclass Classification – Attack Identification

The selected XGBoost algorithm was also trained to classify nine attack categories.

| Metric | Test Result |
|---|---:|
| Accuracy | 96.38% |
| Macro F1-score | 85.96% |

Attack categories:
- Backdoor
- DDoS
- DoS
- Injection
- MITM
- Password
- Ransomware
- Scanning
- XSS

## Technologies Used

- **Programming Language:** Python
- **Data Processing:** Pandas, NumPy
- **Machine Learning:** Scikit-learn, XGBoost
- **Visualization:** Matplotlib, Seaborn
- **Model Persistence:** Joblib
- **Environment:** Jupyter Notebook / Kaggle

## Project Structure

```text
Network-Intrusion-Detection-System/
│
├── nids_echantillon_3m.ipynb
├── data/
│   └── NF-ToN-IoT-v3.csv
│
├── models/
│   └── nids_modele_selectionne_3m.joblib
│
├── README.md
└── .gitignore
```

*Note: The dataset and trained model artifacts are not necessarily included in the GitHub repository.*

## How to Run

1. Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
cd YOUR_REPOSITORY
```

2. Install the required libraries:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn xgboost joblib
```

3. Download the NF-ToN-IoT-v3 dataset and place it in the appropriate data directory.

4. Update the dataset and project paths in the notebook if necessary.

5. Open and execute the notebook:

```bash
jupyter notebook nids_echantillon_3m.ipynb
```

**Note:** The notebook processes a large dataset and performs multiple cross-validation experiments. Sufficient RAM and computational resources are recommended.

## Key Objectives

- Detect malicious network traffic accurately.
- Reduce false positive alerts.
- Address class imbalance using stratified sampling and sampling weights.
- Compare different machine learning algorithms.
- Identify multiple categories of network attacks.
- Develop a reproducible and data-driven intrusion detection approach.

---

**Project Focus:** Cybersecurity | Network Traffic Analysis | Machine Learning | Intrusion Detection | CRISP-DM
