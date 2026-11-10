# Concrete Compressive Strength Prediction

** Data Mining, Modeling and Analytics, Project 2 

This project predicts the compressive strength of concrete (MPa) from its mix ingredients and curing age. It compares a mean-prediction baseline with linear, polynomial and tree-based regression models, examines where the models make errors, and discusses whether the results can support causal claims.

## Dataset
- **Size:** 1,030 laboratory tests, 8 predictors, 1 target
- **Predictors:** cement, blast furnace slag, fly ash, water, superplasticizer, coarse aggregate, fine aggregate (all kg/m³) and age (days)
- **Target:** compressive strength (MPa)

## Repository structure

```
.
├── README.md
├── requirements.txt
├── data/
│   └── Concrete_Data.csv
├── notebooks/
│   └── Project2_Concrete_Strength.ipynb
└── reports/
    └── Concrete_Strength_Report.pdf
```

## How to run

```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
pip install -r requirements.txt
jupyter notebook notebooks/Project2_Concrete_Strength.ipynb
```

Run all cells from top to bottom. The notebook reads the data from `data/Concrete_Data.csv`.

## Method

1. **Exploration.** Checked missing values, duplicates, zero values, outliers, skewness and correlations.
2. **Preparation.** Removed 25 exact duplicate rows (1,005 rows remained). Kept zeros and outliers because they are physically valid. Added log(age) and the water/cement ratio for linear models. Standardised inputs inside pipelines to prevent data leakage.
3. **Evaluation.** Held out 20% of the data as a test set and used 5-fold cross-validation on the remaining 80%.
4. **Models.** Mean baseline, linear regression, linear regression with engineered features, polynomial ridge regression, random forest and gradient boosting.
5. **Diagnostics.** Residual analysis by curing age, strength level and mix type, plus a stability check over 10 random splits.

## Results

| Model | Test RMSE (MPa) | Test MAE (MPa) | Test R² |
| --- | --- | --- | --- |
| Mean baseline | 17.30 | 13.97 | 0.00 |
| Linear (raw features) | 11.19 | 8.90 | 0.58 |
| Linear (log age + w/c ratio) | 7.59 | 5.78 | 0.81 |
| Polynomial degree 2 (ridge) | 6.18 | 4.47 | 0.87 |
| Random forest | 5.10 | 3.44 | 0.91 |
| **Gradient boosting** | **4.26** | **2.68** | **0.94** |

**Key findings**

- Every model beat the mean baseline. Gradient boosting performed best and kept its lead across 10 random splits.
- Domain-based features (log age and water/cement ratio) raised the linear model's R² from 0.58 to 0.81.
- All models were least accurate on high-strength mixes.
- The models show predictive associations only. They do not show that changing cement content causes a change in strength.

## Author

Kwera Stella M
