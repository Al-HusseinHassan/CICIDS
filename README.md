# CICIDS2017 Network Intrusion Detection

A machine learning project for analyzing network traffic and detecting/classifying cyber attacks using the **CICIDS2017 dataset**.

The project focuses on data cleaning, exploratory data analysis (EDA), feature engineering, visualization, and machine learning for network intrusion detection.

---

## Project Overview

Cybersecurity systems generate large amounts of network traffic data, making automated intrusion detection an important application of machine learning.

In this project, the **CICIDS2017** dataset is analyzed to understand network traffic patterns and identify characteristics associated with different types of cyber attacks.

### Main Objectives

* Analyze and understand network traffic data.
* Perform comprehensive data cleaning.
* Identify missing values, duplicates, infinite values, and abnormal observations.
* Explore relationships between network features and attack types.
* Visualize traffic patterns and attack distributions.
* Prepare the dataset for machine learning.
* Train machine learning models for intrusion detection/classification.
* Evaluate model performance using appropriate metrics.

---

## Dataset

The project uses the **CICIDS2017** dataset, a widely used benchmark dataset for intrusion detection research.

The dataset contains labeled network flows representing both **benign traffic** and several types of cyber attacks.

### Dataset Statistics

| Property      |                          Value |
| ------------- | -----------------------------: |
| Total Rows    |                      2,830,743 |
| Total Columns |                             81 |
| Dataset Type  |                Network Traffic |
| Target        |                    Attack Type |
| Classes       | Benign + Multiple Attack Types |

### Attack Categories

The dataset contains several attack families, including:

* DoS
* DDoS
* PortScan
* Brute Force
* Web Attacks
* Infiltration
* Bot
* Heartbleed
* Other malicious traffic

---

## Exploratory Data Analysis

The EDA stage focuses on understanding the structure and quality of the network traffic data.

### Data Quality Analysis

The following issues were investigated:

* Missing values
* Duplicate records
* Infinite values
* Constant-value features
* Invalid numerical values
* Zero-duration flows
* Highly skewed features
* Outliers
* Class imbalance

For example, the dataset contains a constant feature:

```text
bwd_psh_flags
```

which provides no useful variation for machine learning.

The dataset also contains duplicate observations and missing values in features such as:

```text
flow_bytes/s
```

---

## Data Cleaning

Several preprocessing steps were performed to improve data quality.

### Main Cleaning Steps

1. Identify missing values.
2. Detect and remove duplicate observations.
3. Handle infinite values.
4. Investigate invalid numerical values.
5. Analyze zero-duration flows.
6. Remove features with no useful variance.
7. Clean and standardize attack labels.
8. Create meaningful attack categories.
9. Prepare numerical features for modeling.

Special attention was given to features such as:

* `flow_duration`
* `flow_bytes/s`
* `flow_iat_min`
* `total_length_of_bwd_packets`

because their distributions contained unusual or extreme values.

---

## Data Analysis

The project investigates how network traffic characteristics differ between benign and malicious traffic.

Examples of analyzed relationships include:

* Packet counts
* Forward/backward traffic
* Flow duration
* Packet lengths
* Inter-arrival times
* Bytes transferred
* Flow rates
* Attack categories

Visualizations were created using:

* Matplotlib
* Seaborn

Examples include:

* Distribution plots
* Histograms
* Box plots
* Count plots
* Scatter plots
* Correlation heatmaps

---

## Machine Learning

After completing the EDA and preprocessing stages, the cleaned dataset is prepared for machine learning.

The project explores machine learning approaches for:

### 1. Binary Classification

Classify network traffic into:

```text
Benign
Attack
```

### 2. Multiclass Classification

Identify the specific attack category associated with malicious traffic.

This allows the system to move from simply detecting an intrusion to understanding **what type of attack is occurring**.

---

## Technologies Used

### Programming Language

* Python

### Data Analysis

* NumPy
* Pandas

### Visualization

* Matplotlib
* Seaborn

### Machine Learning

* Scikit-learn
* XGBoost

### Development Environment

* Jupyter Notebook
* Anaconda

---

## Key Findings

During the analysis, several important characteristics of the dataset were identified:

* The dataset contains significant class imbalance.
* A large number of duplicate observations are present.
* Some features contain missing or infinite values.
* `flow_duration` has an important relationship with rate-based features.
* Some network features have highly skewed distributions.
* Attack and benign traffic exhibit different statistical patterns.
* Some features contain little or no useful information and can be removed.
* Feature relationships reveal potential multicollinearity that should be considered during modeling.

---

## Model Evaluation

Models are evaluated using multiple metrics rather than accuracy alone.

### Evaluation Metrics

* Accuracy
* Precision
* Recall
* F1-score
* Confusion Matrix

For an intrusion detection system, **recall is particularly important**, because failing to detect a real attack can have serious consequences.

---

## Future Improvements

Possible improvements to the project include:

* Advanced feature selection.
* Hyperparameter optimization.
* Handling class imbalance with different sampling strategies.
* Testing additional ensemble models.
* Comparing tree-based models with neural networks.
* Performing cross-validation.
* Evaluating computational efficiency.
* Developing a real-time intrusion detection pipeline.
* Deploying the trained model as an API.
* Integrating the model into a cybersecurity monitoring system.

---

## Learning Outcomes

Through this project, I practiced:

* Real-world dataset analysis.
* Data cleaning and preprocessing.
* Exploratory data analysis.
* Statistical analysis.
* Data visualization.
* Feature engineering.
* Feature selection.
* Classification.
* Model evaluation.
* Handling imbalanced datasets.
* Applying machine learning to cybersecurity.

---

## Author

**Al-Hussein Hassan Hendawy**

Computer Science & Artificial Intelligence Student

Interested in:

* Data Science
* Machine Learning
* Artificial Intelligence
* Cybersecurity
* Intelligent Systems

---

## 📜 Dataset Reference

The project is based on the **CICIDS2017** dataset developed by the Canadian Institute for Cybersecurity.
You can Download it from the official website <a href="https://www.unb.ca/cic/datasets/ids-2017.html">here</a>
