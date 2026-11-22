# Cervical Cancer Risk Prediction

Predicting biopsy-confirmed cervical cancer from patient demographics, habits and medical history using an XGBoost classifier.

## Dataset
- **858 patients** from Hospital Universitario de Caracas, Venezuela
- 36 raw columns covering demographics, sexual and reproductive history, smoking, hormonal contraceptives, IUD use, STD history and screening test results
- **Target:** `Biopsy` (about 6.4% positive, so heavily imbalanced)

## Workflow
1. **Cleaning:** replaced `?` with NaN, dropped two sparsely filled columns (time since first/last STD diagnosis), converted columns to numeric and imputed missing values with column means
2. **EDA:** missing-value heatmaps, correlation matrix heatmap and feature histograms
3. **Preparation:** standardized features with `StandardScaler`; split into train (80%), test (10%) and validation (10%)
4. **Model:** `XGBClassifier(learning_rate=0.1, max_depth=50, n_estimators=100)`
5. **Evaluation:** accuracy, classification report and confusion matrix

## Results
| Metric | Score |
|---|---|
| Train accuracy | 99.6% |
| Test accuracy | 97.7% |

Test-set classification report (86 patients):

| Class | Precision | Recall | F1 | Support |
|---|---|---|---|---|
| 0 (negative) | 0.98 | 1.00 | 0.99 | 81 |
| 1 (positive) | 1.00 | 0.60 | 0.75 | 5 |

Accuracy looks high because most patients are negative. The model missed 2 of the 5 positive cases in the test set. No random seed is fixed, so exact numbers vary between runs.

## Limitations and next steps
- Evaluate with PR-AUC and ROC-AUC, since accuracy is misleading on imbalanced data
- Use a stratified split and cross-validation, as the test set has only 5 positive cases
- Fit imputation and scaling on the training set only, to avoid leakage
- Try class weighting (`scale_pos_weight`) and a Logistic Regression baseline
- Other screening-test columns (Hinselmann, Schiller, Citology) are included as features and may inflate results

## Project Structure
```
├── Cervical_Cancer_Prediction.ipynb   # analysis and model
├── data/                              # dataset
├── LICENSE
└── README.md
```

## How to Run
```bash
git clone https://github.com/ssb-ctrl/cervical-cancer-risk-prediction.git
cd cervical-cancer-risk-prediction
pip install pandas numpy matplotlib seaborn plotly scikit-learn xgboost jupyter
jupyter notebook
```
