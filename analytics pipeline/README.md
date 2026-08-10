# Module 2 — Analytics Pipeline (`/analytics`)

This module implements the required Titanic analytics workflow from profiling and cleaning through EDA, classification, imbalance handling, hyperparameter tuning, regression, and saving a complete prediction pipeline.

## Required structure

```text
analytics/
├── 01_eda.ipynb
├── 02_modeling.ipynb
├── titanic.csv
├── best_pipeline.joblib
├── model_comparison.csv
├── EDA_RESULTS.md
├── MODELING_RESULTS.md
├── charts/
└── README.md
```

`01_eda.ipynb` is the only place where `sns.load_dataset("titanic")` is called. It immediately saves the loaded dataset to `titanic.csv`, then performs the required cleaning and saves the cleaned DataFrame back to the same committed fallback file.

`02_modeling.ipynb` reads `titanic.csv` with `pd.read_csv()` and never calls `sns.load_dataset()`.

## Installation

From the repository root:

```bash
pip install -r analytics/requirements.txt
```

## Run order

Open Jupyter:

```bash
jupyter notebook
```

Then open:

1. `analytics/01_eda.ipynb`
2. Run every cell from top to bottom.
3. Confirm `titanic.csv`, `charts/`, and `EDA_RESULTS.md` were generated.
4. Commit `titanic.csv` to the repository.
5. Open `analytics/02_modeling.ipynb`.
6. Run every cell from top to bottom.
7. Confirm `best_pipeline.joblib`, `model_comparison.csv`, `MODELING_RESULTS.md`, and the remaining charts were generated.

Do not independently run `sns.load_dataset("titanic")` in the modeling notebook.

## Dataset loading

The raw Titanic dataset is loaded exactly once:

```python
df = sns.load_dataset("titanic")
```

Immediately after loading:

```python
df.to_csv("titanic.csv", index=False)
```

The modeling notebook uses:

```python
df = pd.read_csv("titanic.csv")
```

This provides the required offline fallback.

## Missing-value strategy

The notebooks print the exact measured missing percentage for every affected column before cleaning.

The required threshold is applied as follows:

- Under 5% missing: drop affected rows.
- 5%–30% missing: impute.
- Above 30% missing: explicitly decide whether to drop the column or encode missing as a category.

For the standard Seaborn Titanic dataset, the expected strategy is:

- `age`: median imputation because its missingness is between 5% and 30%.
- `embarked`: drop affected rows because missingness is below 5%.
- `embark_town`: drop affected rows because missingness is below 5%; it is also redundant with `embarked`.
- `deck`: drop the column because missingness is above 30% and reliable imputation would be inappropriate.

The notebook prints the actual percentages used for these decisions, so the submitted report contains measured values rather than assumptions.

## EDA requirements covered

`01_eda.ipynb` contains:

- `df.info()`
- `df.describe()`
- `df.shape`
- missing-value percentages
- missing-value handling decisions
- age histogram
- age box plot
- fare histogram
- fare box plot
- IQR outlier counts for age and fare
- fare mean, median, and mode
- fare skewness conclusion
- survival by sex
- survival by pclass
- survival by sex and pclass
- boolean masking using `&` and `|`
- exactly the required six-column correlation matrix:
  `survived`, `pclass`, `age`, `sibsp`, `parch`, `fare`
- correlation heatmap
- the two strongest absolute off-diagonal correlations
- four distinct multivariate charts
- a written interpretation for every multivariate chart
- exploratory standardization of age and fare
- before/after standardization summary

The exploratory standardization is not reused by the modeling pipeline.

## Modeling requirements covered

`02_modeling.ipynb` contains:

- stratified train/test split before preprocessing
- class-balance report
- training-only preprocessing
- median imputation for numeric model features
- most-frequent imputation for categorical model features
- one-hot encoding of `sex` and `embarked`
- `StandardScaler` for numeric model features
- Logistic Regression
- Decision Tree
- Random Forest
- Decision Tree visualization with `plot_tree`
- confusion matrices
- accuracy
- precision
- recall
- F1
- ROC curves
- AUC
- three-way imbalance comparison:
  - baseline
  - `class_weight="balanced"`
  - SMOTE
- SMOTE applied only to the training data through an `imblearn` pipeline
- Random Forest `GridSearchCV`
- tuning of `n_estimators`, `max_depth`, and `max_features`
- `RandomForestClassifier(oob_score=True, ...)`
- best parameters and OOB score
- multivariate linear regression for `fare`
- MAE
- RMSE
- R²
- Adjusted R²
- residual plot
- heteroscedasticity conclusion
- combined model comparison table
- final classifier recommendation
- complete fitted preprocessing + estimator pipeline saved with `joblib.dump()`
- reload test using `joblib.load()`

## Important leakage rule

All model preprocessing is contained inside scikit-learn `Pipeline` / `ColumnTransformer` objects.

The preprocessing steps are fitted on `X_train` only. The test data is never used to fit the imputer, encoder, scaler, or model.

SMOTE is placed after the preprocessing step inside an `imblearn` pipeline, so oversampling occurs only during training.

## Output files

The EDA notebook generates:

```text
analytics/titanic.csv
analytics/EDA_RESULTS.md
analytics/charts/01_age_histogram.png
analytics/charts/02_age_boxplot.png
analytics/charts/03_fare_histogram.png
analytics/charts/04_fare_boxplot.png
analytics/charts/05_correlation_heatmap.png
analytics/charts/06_survival_by_sex.png
analytics/charts/07_survival_by_sex_and_class.png
analytics/charts/08_age_by_survival.png
analytics/charts/09_fare_by_survival.png
```

The modeling notebook generates:

```text
analytics/best_pipeline.joblib
analytics/model_comparison.csv
analytics/MODELING_RESULTS.md
analytics/charts/10_confusion_matrices.png
analytics/charts/11_roc_curves.png
analytics/charts/12_decision_tree.png
analytics/charts/13_regression_residuals.png
```

## Final Git submission

The `/analytics` folder belongs inside the same project repository as the other modules.

The repository history must also satisfy the project-wide requirement that a feature branch is created, committed to at least twice, and merged back into `main`.

Example:

```bash
git checkout main
git checkout -b feature/analytics-pipeline

git add analytics/
git commit -m "feat: add Titanic EDA pipeline"

# Make a legitimate second improvement, then:
git add analytics/
git commit -m "feat: add predictive modeling pipeline"

git checkout main
git merge feature/analytics-pipeline

git push origin main
git push -u origin feature/analytics-pipeline
```

Verify:

```bash
git log --oneline --graph --all
```

## Submission

Submit the URL of the single GitHub repository containing `/analytics`.

The evaluator should be able to open:

```text
<repository>/analytics/
```

and find the notebooks, committed `titanic.csv`, saved/generated model artifact, charts, reports, and README.
