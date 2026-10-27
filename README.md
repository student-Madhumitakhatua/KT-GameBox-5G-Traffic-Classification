# KT GameBox 5G Traffic Classification Using Machine Learning

An end-to-end Machine Learning project for classifying TCP and UDP network traffic using the KT GameBox 5G traffic dataset.

This project covers large-scale CSV processing, protocol analysis, data cleaning, dataset balancing, feature engineering, feature selection, model training, and performance evaluation.

## Project Overview

The objective of this project is to classify network packets into two protocol classes:

- **UDP (User Datagram Protocol):** `0`
- **TCP (Transmission Control Protocol):** `1`

The original dataset consists of 10 CSV files containing 38,096,046 packet records. The files were merged, analyzed, and processed to create a balanced dataset containing 1,000,000 TCP and UDP samples.

Four Machine Learning models were compared: Logistic Regression, Linear SVM, Decision Tree, and Random Forest.

The Decision Tree achieved an accuracy of **99.9915%** on the prepared test dataset.

> Note: This is held-out test-set performance. Since source and destination IP addresses are strongly associated with protocol labels, further evaluation on unseen sessions and network environments is necessary to assess generalization.

## Project Objectives

- Merge 10 large CSV files.
- Analyze the distribution of network protocols.
- Select TCP and UDP traffic for binary classification.
- Create a balanced dataset of 1,000,000 samples.
- Clean and preprocess the data.
- Investigate potential target leakage.
- Encode categorical IP addresses using One-Hot Encoding.
- Apply feature scaling and statistical feature selection.
- Train and compare multiple Machine Learning models.
- Evaluate models using accuracy, precision, recall, F1-score, and a confusion matrix.

## Dataset Information

### Original Dataset

- **Dataset:** KT GameBox 5G Traffic
- **Input files:** 10 CSV files
- **Total original records:** 38,096,046
- **Original columns:** `No.`, `Time`, `Source`, `Destination`, `Protocol`, `Length`, `Info`

### Balanced Dataset

| Protocol | Label | Samples |
|---|---:|---:|
| UDP | 0 | 500,000 |
| TCP | 1 | 500,000 |
| **Total** | — | **1,000,000** |

The balanced dataset is saved as `KT_GameBox_TCP_UDP_Balanced.csv`.

## Project Structure

```text
KT_GameBox/
│
├── KT_GameBox_1.csv
├── KT_GameBox_2.csv
├── KT_GameBox_3.csv
├── KT_GameBox_4.csv
├── KT_GameBox_5.csv
├── KT_GameBox_6.csv
├── KT_GameBox_7.csv
├── KT_GameBox_8.csv
├── KT_GameBox_9.csv
├── KT_GameBox_10.csv
│
├── KT_GameBox_Merged.csv
├── KT_GameBox_TCP_UDP_Balanced.csv
├── KT_GameBox_ML_Process_Explanation.txt
├── test1.ipynb
└── README.md
```

## Project Workflow

1. Merge the 10 original CSV files.
2. Analyze protocol distribution.
3. Select TCP and UDP records.
4. Create a balanced dataset with 500,000 samples per class.
5. Check for missing values and duplicate rows.
6. Remove the `No.` and `Info` columns.
7. Analyze time-based features for possible leakage.
8. Remove `hour`, `minute`, and `second` from the final model.
9. Apply One-Hot Encoding to source and destination IP addresses.
10. Split the data into training and testing sets using an 80/20 stratified split.
11. Apply StandardScaler.
12. Perform feature selection using the ANOVA F-test.
13. Train and compare Machine Learning models.
14. Evaluate the selected model using a confusion matrix.

## Data Preprocessing and Feature Engineering

### Data Cleaning

The prepared balanced dataset was checked for missing values, duplicate rows, and data types. The recorded checks found zero missing values and zero duplicate rows.

The `No.` column was removed because it is a packet identifier. The `Info` column was removed because its packet descriptions could reveal information associated with the target protocol.

### Time-Based Leakage Analysis

The `Time` column was initially converted into datetime format, and `hour`, `minute`, and `second` were extracted.

Exploratory analysis showed that certain hours in the prepared data were associated exclusively with TCP traffic. These time-based features were therefore removed from the final model.

### One-Hot Encoding

`Source` and `Destination` were treated as categorical features rather than ordered numerical values.

The prepared dataset contained:

- 8 unique source IP addresses
- 8 unique destination IP addresses
- 1 numerical feature: `Length`

One-Hot Encoding produced 17 features before feature selection.

### Train-Test Split

| Dataset | Total Samples | UDP | TCP |
|---|---:|---:|---:|
| Training | 800,000 | 400,000 | 400,000 |
| Testing | 200,000 | 100,000 | 100,000 |

### Feature Scaling and Selection

StandardScaler was fitted on the training data and then used to transform the training and testing sets.

The ANOVA F-test (`sklearn.feature_selection.f_classif`) was used for feature selection. Two features with reported p-values greater than 0.05 were removed, reducing the feature count from 17 to 15.

## Machine Learning Models

The following models were evaluated:

1. **Logistic Regression** — Baseline classification model.
2. **Linear SVM** — Linear decision-boundary classifier.
3. **Decision Tree** — Interpretable model based on feature-based decision rules.
4. **Random Forest** — Ensemble of decision trees.

K-Nearest Neighbors was also attempted, but execution was too slow for the dataset size and was excluded from the final comparison.

## Model Performance

| Model | Accuracy | Precision | Recall | F1-Score |
|---|---:|---:|---:|---:|
| Logistic Regression | 92.77% | 91.48% | 94.32% | 92.88% |
| Linear SVM | 92.76% | 91.48% | 94.32% | 92.88% |
| Decision Tree | 99.99% | 99.99%* | 99.99%* | 99.99%* |
| Random Forest | 99.99% | 99.99%* | 99.99%* | 99.99%* |

\* Rounded reported results.

### Decision Tree Confusion Matrix

| Actual Class | Predicted UDP | Predicted TCP |
|---|---:|---:|
| UDP | 99,993 | 7 |
| TCP | 10 | 99,990 |

- Total test samples: 200,000
- Correct predictions: 199,983
- Incorrect predictions: 17
- Exact accuracy: **99.9915%**

The Decision Tree was selected because it achieved the same reported rounded performance as Random Forest while offering a simpler model structure.

## Key Findings

- Successfully merged 10 CSV files containing more than 38 million packet records.
- Identified the original imbalance between UDP and TCP traffic.
- Created a balanced dataset containing 1 million samples.
- Applied preprocessing and feature engineering techniques.
- Compared four Machine Learning classification algorithms.
- Achieved 99.9915% Decision Tree accuracy on the prepared test set.
- Identified IP-address associations as an important potential limitation for generalization.

## Limitations and Future Improvements

The high test accuracy should not automatically be interpreted as equivalent performance on unseen networks. Source and destination IP addresses are strongly associated with protocol labels in this dataset.

Future improvements include:

- Use session-aware or environment-aware train-test splitting.
- Test the model on completely unseen network sessions.
- Compare performance with and without IP address features.
- Investigate flow-level features such as packet counts, flow duration, and inter-arrival times.
- Evaluate models on data retaining the original protocol distribution.
- Compare inference time and model size in addition to classification metrics.

## Technologies Used

- Python
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- Seaborn
- Jupyter Notebook
- VS Code
- Git and GitHub

## Installation and Usage

### 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/KT_GameBox.git
cd KT_GameBox
```

Replace `<your-username>` with your GitHub username and use your actual repository name if it differs.

### 2. Install Dependencies

```bash
pip install pandas numpy scikit-learn matplotlib seaborn jupyter
```

### 3. Run the Notebook

```bash
jupyter notebook test1.ipynb
```

Execute the notebook cells in order. The original and processed CSV files should be available at the file paths expected by your notebook.

**Dataset note:** The original and merged CSV files are large. If you do not intend to distribute them, consider excluding them from Git and explaining how to obtain the dataset. GitHub has a 100 MB per-file limit for ordinary Git repositories, and large files may require Git LFS or external dataset storage.

## Conclusion

This project demonstrates an end-to-end Machine Learning workflow for TCP and UDP traffic classification using KT GameBox 5G network traffic data.

The workflow includes large-scale data processing, class balancing, preprocessing, feature engineering, feature selection, model comparison, and evaluation.

Decision Tree achieved the highest reported level of performance in the experiment, with an exact test accuracy of 99.9915%. Further testing on unseen sessions and environments is needed to assess real-world generalization.

## Author

**Madhumita Khatua**  
BCA, Brainware University

**Project Focus:** Machine Learning | Network Traffic Classification | Data Preprocessing | Feature Engineering | Model Evaluation
