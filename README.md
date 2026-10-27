# Asthma Exacerbation Risk Prediction Using Categorical Naive Bayes

A data science learning project exploring whether clinical indicators, spirometry tests, and patient symptoms can predict asthma exacerbations.

---

## What This Project Does

This project studies a clinical dataset of pediatric asthma patients monitored over time. The goal is to evaluate whether a machine learning classifier—specifically **Categorical Naive Bayes**—can identify when a patient is experiencing an asthma exacerbation based on their clinical and symptom profile.

### Workflow
1. **Data Exploration & Cleaning:** Explored 195 observations across 65 patients from longitudinal clinical monitoring.
2. **Feature Selection:** Selected 22 categorical clinical, spirometric, and symptom indicators, excluding patient identifiers (`id`) and visit numbers (`t`).
3. **Patient-Level Split:** Split data by unique patient IDs (80% train / 20% test) to prevent observations from the same patient from leaking into both sets.
4. **Encoding:** Fitted categorical and target encoders strictly on the training set, with logic to handle unseen test categories.
5. **Model Training:** Trained a `CategoricalNB` classifier using scikit-learn.
6. **Baseline Comparison:** Benchmarked against a majority-class baseline to examine performance on imbalanced data.
7. **Risk Probability Export & Dashboard:** Exported predicted risk probabilities to CSV and built an interactive Power BI dashboard (`dashboard.pbix`).

---

## Results Summary

Evaluated on the held-out test set (**38 observations from 13 patients**):

| Metric | Categorical Naive Bayes | Majority Baseline |
| :--- | :---: | :---: |
| **Accuracy** | **76.3%** | 81.6% |
| **Exacerbation Precision** | **33.3%** | 0.0% |
| **Exacerbation Recall** | **28.6%** (2 of 7 detected) | 0.0% (0 of 7 detected) |
| **Exacerbation F1-Score** | **30.8%** | 0.0% |

### Key Takeaway
The majority baseline has a higher overall accuracy (81.6%) simply by predicting "No Exacerbation" for every case. However, it detects **0%** of actual exacerbations. The Naive Bayes model detects some exacerbations (28.6% recall), demonstrating why accuracy alone is misleading for imbalanced healthcare datasets.

---

## Repository Structure

```text
├── README.md                      # Project overview and quickstart guide
├── requirements.txt               # Minimal Python dependencies
├── asthma_attack_pred.ipynb       # Main Jupyter notebook (complete workflow)
├── documentation.md               # Detailed technical documentation & decisions
├── biology.md                     # Domain notes on clinical & biological variables
├── dashboard.pbix                 # Power BI reporting dashboard
├── presentation.pptx              # Summary presentation slides
├── data/
│   ├── asthma_exacerbation_dataset.xlsx  # Original clinical dataset (195 rows)
│   └── asthma_prediction_results.csv     # Exported test predictions and risk scores
├── sources/
│   └── 13104_2018_Article_3621.pdf       # Published study paper (Spyroglou et al.)
└── dropped/                       # Archived earlier exploratory work (AAMOS-00)
    └── README.md                  # Explanation of the archived experiment
```

---

## How to Run This Project

### 1. Prerequisites
- Python 3.10+ (tested on Python 3.13)
- Power BI Desktop (optional, to view the interactive `.pbix` dashboard)

### 2. Setup Environment
Clone the repository and install the minimal dependencies:
```bash
git clone https://github.com/dumbgesh/asthma-attack-prediction.git
cd asthma-attack-prediction
pip install -r requirements.txt
```

### 3. Run the Notebook
Open and run the notebook in Jupyter or VS Code:
```bash
jupyter notebook asthma_attack_pred.ipynb
```
Running the notebook will:
- Process the dataset in `data/asthma_exacerbation_dataset.xlsx`.
- Reproduce the train/test split, model training, and evaluation metrics.
- Regenerate `data/asthma_prediction_results.csv`.

### 4. View the Power BI Dashboard
Open [`dashboard.pbix`](dashboard.pbix) in Power BI Desktop to inspect:
- Performance KPI cards (Accuracy, Exacerbation Recall, Actual Cases).
- Actual vs. Predicted comparison chart.
- Predicted risk score bands and scatter distribution.
- Test observation detail table.

---

## Tech Stack
* **Language & Analysis:** Python, Pandas, NumPy, scikit-learn
* **Environment:** Jupyter Notebook
* **Visualization & Reporting:** Power BI Desktop

---

## Dataset Reference

The dataset used in this project is from the following published research:

> Spyroglou, I. I., Spöck, G., Rigas, A. G., & Paraskakis, E. N. (2018). *Evaluation of Bayesian classifiers in asthma exacerbation prediction after medication discontinuation*. **BMC Research Notes**, 11(1), 522.  
> [https://doi.org/10.1186/s13104-018-3621-1](https://doi.org/10.1186/s13104-018-3621-1)

---
## Disclaimer

This repository was created as an educational data science project by a student learner. The model and predicted probabilities are **not clinically validated** and should not be used for medical diagnosis, clinical decision-making, or treatment planning.
