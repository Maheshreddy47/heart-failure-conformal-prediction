# Uncertainty-Aware Heart Failure Survival Prediction
## A Conformal Prediction Framework for Safe Clinical AI

| Detail | Value |
|---|---|
| Student | Mahesh Singireddy |
| Student ID | c5050786 |
| Module | Research Skills for Computing (55-710248) |
| Programme | MSc Computing |
| Institution | Sheffield Hallam University |
| Supervisor | Dr. Olamilekan Shobayo |
| Submission | August 2026 |

---

## Project Summary

This repository contains the complete, reproducible Python pipeline for uncertainty-aware heart failure (HF) survival prediction. The pipeline integrates:

- Random Forest classifier as the primary prediction model
- MAPIE SplitConformalClassifier for uncertainty quantification with finite-sample coverage guarantees
- SHAP TreeExplainer for global and local explainability
- pytest unit tests covering TC1 to TC8 as documented in the written report

### Key Results

| Classifier | AUROC | F1-Score | Accuracy |
|---|---|---|---|
| Random Forest (selected) | 0.894 | 0.737 | 0.833 |
| Logistic Regression | 0.864 | | |
| Gradient Boosting | 0.837 | | |

| Conformal Metric | Value |
|---|---|
| Target Coverage (alpha = 0.05) | 95% |
| Empirical Coverage | 93.33% |
| Confident Predictions (set size 1) | 43 / 60 |
| Ambiguous Predictions (set size 2) | 17 / 60 |

Top SHAP predictors: time (0.198), ejection_fraction (0.068), creatinine_sodium_ratio (0.064), serum_creatinine (0.064)

---

## Repository Structure

```
heart-failure-conformal-prediction/
│
├── README.md
├── requirements.txt
├── heart_failure_pipeline.ipynb
└── heart_failure_clinical_records.csv
```

---

## How to Run

### Step 1 - Clone the Repository

```bash
git clone https://github.com/YOUR-USERNAME/heart-failure-conformal-prediction.git
cd heart-failure-conformal-prediction
```

### Step 2 - Create a Virtual Environment (Recommended)

```bash
python -m venv venv

# On Windows:
venv\Scripts\activate

# On macOS / Linux:
source venv/bin/activate
```

### Step 3 - Install All Dependencies

```bash
pip install -r requirements.txt
```

This installs the exact pinned versions used during development, ensuring full reproducibility (Semmelrock et al., 2025).

### Step 4 - Launch the Notebook

```bash
jupyter notebook heart_failure_pipeline.ipynb
```

Open the URL shown in your terminal, then run all cells from top to bottom using Cell > Run All.

### Step 5 - Run the Unit Tests

```bash
pytest
```

All eight system test cases (TC1 to TC8) should pass, verifying data loading shape, SMOTE balance, partition ratios, AUROC threshold, and conformal coverage.

---

## Dataset

UCI Heart Failure Clinical Records Dataset

- Records: n = 299
- Features: 13 clinical follow-up variables
- Target: DEATH_EVENT (0 = Survived, 1 = Deceased)
- Licence: Creative Commons CC BY 4.0
- Source: https://archive.ics.uci.edu/dataset/519/heart+failure+clinical+records
- Citation: Chicco, D., and Jurman, G. (2020). BMC Medical Informatics and Decision Making, 20, Article 16.

The dataset is included in this repository for reproducibility purposes under the CC BY 4.0 licence.

---

## Pipeline Architecture

The pipeline follows four sequential stages:

```
Stage 1 - Data Ingestion and Preprocessing
    Load UCI CSV, validate 299 x 13, confirm 0 missing values
    Stratified split: Train (60%), Calibration (20%), Test (20%)
    SMOTE on training partition only, 162 samples per class

Stage 2 - Classifier Training
    Train RF, LR, GB on SMOTE-balanced training set
    Evaluate AUROC, F1, Accuracy on test set
    Select Random Forest (AUROC = 0.894) as primary model

Stage 3 - Conformal Prediction (Uncertainty Quantification)
    MAPIE SplitConformalClassifier fitted on calibration set
    alpha = 0.05 (target 95% coverage)
    Empirical coverage = 93.33% on test set (n = 60)

Stage 4 - Explainability (SHAP)
    SHAP TreeExplainer on Random Forest
    Global: beeswarm summary plot and mean SHAP bar chart
    Local: waterfall plot for individual patient predictions
```

---

## System Test Cases

| ID | Description | Expected | Actual | Status |
|---|---|---|---|---|
| TC1 | Data loading shape | (299, 13) | (299, 13) | PASS |
| TC2 | No missing values | 0 nulls | 0 nulls | PASS |
| TC3 | Stratified split sizes | 239 / 30 / 30 | 239 / 30 / 30 | PASS |
| TC4 | SMOTE class balance | 162 per class | 162 per class | PASS |
| TC5 | RF AUROC threshold | > 0.80 | 0.894 | PASS |
| TC6 | Conformal coverage range | 93% to 97% | 93.33% | PASS |
| TC7 | SHAP outputs generated | 60 outputs | 60 produced | PASS |
| TC8 | Repo runs in clean env | No errors | Confirmed | PASS |

---

## Ethics and Governance

- Ethics Approval: UREC2 (Low Risk), approved by Dr. Olamilekan Shobayo, 16 July 2026
- Data: Fully anonymised secondary data, no participant recruitment or personal data collection
- GDPR: The UCI dataset contains no personally identifiable information
- Licence: CC BY 4.0, publicly available for academic research use
- EU AI Act: This pipeline is assessed against Article 9 principles on uncertainty disclosure and risk management. It is a research prototype only and is not a regulated clinical device

---

## Key References

- Chicco, D., and Jurman, G. (2020). Machine learning can predict survival of patients with heart failure. BMC Medical Informatics and Decision Making, 20, Article 16.
- Collins, G. S., et al. (2024). TRIPOD+AI statement. BMJ, 385, e078378.
- Fayyad, J., Alijani, S., and Najjaran, H. (2024). Empirical validation of conformal prediction. Computer Methods and Programs in Biomedicine, 253, Article 108231.
- Ponce-Bobadilla, A. V., et al. (2024). Practical guide to SHAP analysis. Clinical and Translational Science, 17(11), e70056.
- Semmelrock, H., et al. (2025). Reproducibility in machine-learning-based research. AI Magazine, 46(2), e70002.
- Yang, M., et al. (2024). Development and validation of an interpretable conformal predictor. Journal of Medical Internet Research, 26, e50369.

---

## Reproducibility Statement

This repository is maintained in accordance with the reproducibility best practices identified by Semmelrock et al. (2025). All package versions are pinned in requirements.txt. A single Jupyter Notebook contains the complete pipeline from raw data loading through to final SHAP outputs. No external API keys, cloud credentials, or proprietary data are required.
