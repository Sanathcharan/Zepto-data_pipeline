
# EDA Results

## Dataset profile
- Cleaned shape: 889 rows x 14 columns
- Age missing before cleaning: 0.00%
- Embarked missing before cleaning: 0.00%
- Embark_town missing before cleaning: 0.00%
- Deck missing before cleaning: 77.22%

## Missing-value decisions
- `age`: 0.00% missing, so median imputation was used because the rate is between 5% and 30%.
- `embarked`: 0.00% missing, so affected rows were dropped because the rate is below 5%.
- `embark_town`: 0.00% missing, so affected rows were dropped because the rate is below 5%; the column is redundant with `embarked`.
- `deck`: 77.22% missing, so the column was dropped because missingness is above 30% and reliable imputation would be inappropriate.

## Outliers and fare distribution
- Age IQR outliers: 65
- Fare IQR outliers: 114
- Fare mean: 32.0967
- Fare median: 14.4542
- Fare mode: 8.0500
- Fare distribution: right-skewed. This conclusion is based on the ordering of the mean, median, and mode.

## Strongest correlations
pclass vs fare: r=-0.5482; sibsp vs parch: r=0.4145

## Multivariate chart interpretations
1. **Survival by sex:** The survival-rate comparison shows a clear difference between female and male passengers. This supports sex as an important predictor of survival.
2. **Survival by sex and passenger class:** Survival varies jointly by sex and passenger class. Passenger class provides additional context beyond the sex-based difference.
3. **Age by survival:** The age distributions overlap between survivors and non-survivors, showing that age is informative but not a perfect separator.
4. **Fare by survival:** Fare provides an additional socioeconomic signal alongside passenger class and can help explain differences in survival.

## Standardization check
The exploratory z-score transformation was applied only as an EDA sanity check. The modeling pipeline performs its own train-only scaling and does not reuse this exploratory transformation.
