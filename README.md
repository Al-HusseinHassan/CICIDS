# CICIDS2017 Network Intrusion Detection

A machine learning project for **network intrusion detection and attack classification** using the **CICIDS2017 dataset**.

The project covers the complete workflow from data loading and preprocessing to exploratory data analysis and machine learning. The main goal is to analyze network traffic and build models capable of distinguishing **benign traffic from attacks** and classifying malicious traffic into major attack categories.

---

## Project Overview

Network intrusion detection is an important cybersecurity application of machine learning. Network traffic contains many characteristics that can help identify abnormal behavior and distinguish different types of attacks.

In this project, I worked with the CICIDS2017 dataset and developed a preprocessing and modeling pipeline consisting of:

* Dataset loading
* Data cleaning
* Label preprocessing
* Invalid-value handling
* Feature analysis
* Exploratory Data Analysis (EDA)
* Feature categorization
* Data normalization
* Binary attack detection
* Multiclass attack classification
* Class balancing using sample weights
* Model evaluation

---

## Dataset

The project uses the **CICIDS2017** network intrusion detection dataset.

The dataset was loaded from multiple CSV files corresponding to different days and attack scenarios, including:

* Monday
* Tuesday
* Wednesday
* Thursday
* Friday

The combined dataset initially contains millions of network-flow records.

After preprocessing, the dataset used for analysis contains:

* **2,827,817 rows**
* **76 columns**

The dataset contains both **benign traffic** and several types of malicious traffic.

---

## Attack Categories

The original `label` column contains individual attack types.

I created a higher-level `major_cat` column to group related attacks into broader categories.

### Major Categories

| Major Category | Examples                                 |
| -------------- | ---------------------------------------- |
| `benign`       | Normal network traffic                   |
| `dos`          | Hulk, GoldenEye, Slowloris, Slowhttptest |
| `ddos`         | DDoS                                     |
| `brute-force`  | FTP-Patator, SSH-Patator                 |
| `web-attack`   | Brute Force, XSS, SQL Injection          |
| `port-scan`    | PortScan                                 |
| `bot`          | Bot                                      |
| `infiltration` | Infiltration                             |
| `heartbleed`   | Heartbleed                               |

I also created a `sub_attack` column to preserve more specific attack information.

For example:

```text
major_cat       sub_attack
--------------------------------
dos             hulk
dos             goldeneye
dos             slowloris
brute-force     ftp-patator
brute-force     ssh-patator
web-attack      brute-force
web-attack      xss
web-attack      sql-injection
```

---

## Classification Tasks

The project investigates two main machine learning tasks.

### 1. Binary Intrusion Detection

The `binary_cat` target represents:

```text
0 → Benign
1 → Attack
```

This model answers:

> **Is this network flow normal or malicious?**

### 2. Multiclass Attack Classification

For malicious traffic, the model predicts the `major_cat` attack category.

This answers:

> **What type of attack is occurring?**

The classification model works with categories such as:

```text
dos
ddos
brute-force
web-attack
port-scan
bot
infiltration
heartbleed
```

---

# Data Preprocessing

## Cleaning Column Names

The original column names contained spaces and inconsistent formatting.

They were standardized by:

* Removing leading/trailing spaces
* Replacing spaces with `_`
* Converting names to lowercase

For example:

```text
" Flow Duration"
```

became:

```text
"flow_duration"
```

---

## Cleaning Labels

Attack labels were normalized by:

* Converting labels to lowercase
* Cleaning inconsistent characters
* Standardizing attack names

For example:

```text
DoS Hulk
```

became:

```text
dos hulk
```

---

## Removing Invalid Values

Several features contained unreasonable negative values.

The following conditions were investigated:

* Negative `flow_duration`
* Negative `flow_iat_min`
* Negative `fwd_header_length`
* Negative `bwd_header_length`

Rows containing invalid values were removed.

For example, the dataset contained:

* **115** rows with negative `flow_duration`
* **2,776** rows with negative `flow_iat_min`
* **35** rows with negative `fwd_header_length`

These rows were removed before continuing the analysis.

---

## Removing Uninformative Features

The following bulk-transfer features were removed because they contained no useful variation in the dataset:

```text
bwd_avg_packets/bulk
fwd_avg_packets/bulk
bwd_avg_bytes/bulk
fwd_avg_bytes/bulk
fwd_avg_bulk_rate
bwd_avg_bulk_rate
```

---

# Exploratory Data Analysis

The EDA stage was used to understand the structure and quality of the dataset before applying machine learning.

The analysis investigated:

* Dataset dimensions
* Data types
* Missing values
* Duplicate records
* Constant features
* Features with very few unique values
* Feature distributions
* Relationships between features
* Attack distributions
* Network traffic characteristics

After preprocessing, the dataset contained:

```text
Rows:    2,827,817
Columns: 76
```

The analysis also identified:

```text
Missing values: Yes
Duplicate values: Yes
Constant feature: bwd_psh_flags
```

Several binary features contained very few unique values, including TCP flag-related features.

---

# Feature Analysis

The project examines different groups of network-flow features.

### Flow Features

Examples:

```text
flow_duration
destination_port
```

### Packet Count Features

```text
total_fwd_packets
total_backward_packets
```

### Packet Length Features

```text
total_length_of_fwd_packets
total_length_of_bwd_packets
fwd_packet_length_max
fwd_packet_length_min
fwd_packet_length_mean
bwd_packet_length_max
bwd_packet_length_min
bwd_packet_length_mean
```

### Flow Rate Features

```text
flow_bytes/s
flow_packets/s
fwd_packets/s
bwd_packets/s
```

### Inter-Arrival Time Features

```text
flow_iat_mean
flow_iat_std
flow_iat_max
flow_iat_min
fwd_iat_mean
fwd_iat_std
bwd_iat_mean
bwd_iat_std
```

### TCP Features

```text
fin_flag_count
syn_flag_count
rst_flag_count
psh_flag_count
ack_flag_count
urg_flag_count
ece_flag_count
cwe_flag_count
```

### Active/Idle Features

```text
active_mean
active_std
active_max
active_min
idle_mean
idle_std
idle_max
idle_min
```

These features describe different characteristics of network communication and provide the information used by the machine learning models.

---

# Machine Learning Pipeline

The machine learning workflow follows these general steps:

```text
CICIDS2017 Dataset
        ↓
Data Loading
        ↓
Data Cleaning
        ↓
Label Processing
        ↓
Invalid Value Removal
        ↓
Feature Preparation
        ↓
Train / Test Split
        ↓
StandardScaler
        ↓
XGBoost
        ↓
Prediction
        ↓
Model Evaluation
```

---

# Models

## Model 1 — Attack Detection

The first model performs binary classification:

```text
Benign vs Attack
```

The model used was:

**XGBoost Classifier**

```python
XGBClassifier(
    random_state=42
)
```

### Result

| Metric   |      Score |
| -------- | ---------: |
| Accuracy | **99.92%** |

The classification report showed approximately perfect precision and recall for both classes on the held-out test set.

---

## Model 2 — Attack Classification

The second model removes benign traffic and focuses only on malicious network flows.

The target is:

```text
major_cat
```

The model uses:

**XGBoost Classifier**

with multiclass classification.

### Result

| Metric      |      Score |
| ----------- | ---------: |
| Accuracy    | **99.97%** |
| Macro F1    |   **0.98** |
| Weighted F1 |   **1.00** |

The very small classes should be interpreted carefully because some categories contain very few test samples.

---

## Model 3 — Class-Balanced Attack Classification

To address class imbalance, balanced sample weights were calculated using:

```python
compute_sample_weight(
    class_weight='balanced',
    y=Y_training2_encoded
)
```

The resulting weights were passed to the XGBoost model during training.

### Result

| Metric      |      Score |
| ----------- | ---------: |
| Accuracy    | **99.98%** |
| Macro F1    |   **0.98** |
| Weighted F1 |   **1.00** |

---

## Model 4 — Multiclass Classification Including Benign Traffic

A final XGBoost model was trained using the complete dataset, including benign traffic, with the `major_cat` target.

Class-balanced sample weights were also used.

### Result

| Metric      |      Score |
| ----------- | ---------: |
| Accuracy    | **99.89%** |
| Macro F1    |   **0.97** |
| Weighted F1 |   **1.00** |

---

# Model Comparison

| Model   | Task                           |   Accuracy |
| ------- | ------------------------------ | ---------: |
| Model 1 | Benign vs Attack               | **99.92%** |
| Model 2 | Attack Classification          | **99.97%** |
| Model 3 | Balanced Attack Classification | **99.98%** |
| Model 4 | Benign + Attack Classification | **99.89%** |

> **Note:** Accuracy is not sufficient by itself for evaluating intrusion-detection systems, especially when classes are highly imbalanced. Precision, recall, F1-score, and the distribution of samples per class should also be considered.

---

# Technologies Used

### Programming Language

* Python

### Data Analysis

* NumPy
* Pandas

### Data Visualization

* Matplotlib
* Seaborn

### Machine Learning

* Scikit-learn
* XGBoost
* imbalanced-learn

### Environment

* Jupyter Notebook
* Anaconda

---

# Key Learning Outcomes

Through this project, I practiced:

* Working with a large real-world cybersecurity dataset
* Data cleaning and preprocessing
* Handling invalid numerical values
* Working with highly imbalanced data
* Feature understanding and analysis
* Exploratory Data Analysis
* Data visualization
* Feature categorization
* Label encoding
* Feature standardization
* Binary classification
* Multiclass classification
* XGBoost
* Class-balanced training
* Model evaluation

---

# Future Improvements

Possible improvements to this project include:

* More advanced feature selection
* Hyperparameter tuning
* Cross-validation
* More detailed confusion-matrix analysis
* Testing additional machine learning algorithms
* More robust evaluation on unseen network environments
* Model explainability using SHAP
* Real-time network traffic classification
* Deployment as an intrusion-detection service

---

# Project Context

This project is part of my broader work in **Artificial Intelligence, Machine Learning, and Cybersecurity**, with the goal of applying machine learning techniques to intelligent cyber-defense systems.

---

## Author

**Al-Hussein Hassan**

Computer Science & Artificial Intelligence Student

**Interests:**

* Data Science
* Machine Learning
* Artificial Intelligence
* Cybersecurity
* Intelligent Systems

# Dataset Reference
The project is based on the CICIDS2017 dataset developed by the Canadian Institute for Cybersecurity.
You can download it from the official website <a href="https://www.unb.ca/cic/datasets/ids-2017.html">here</a> 
