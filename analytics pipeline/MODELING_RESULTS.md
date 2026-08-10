
# Modeling Results

## Class balance
| survived     |   count |   percentage |
|:-------------|--------:|-------------:|
| Not Survived |     549 |      61.7548 |
| Survived     |     340 |      38.2452 |

## Classification comparison
|                     |   Accuracy |   Precision |   Recall |     F1 |    AUC |
|:--------------------|-----------:|------------:|---------:|-------:|-------:|
| Logistic Regression |     0.8146 |      0.7612 |   0.75   | 0.7556 | 0.8609 |
| Decision Tree       |     0.7921 |      0.8039 |   0.6029 | 0.6891 | 0.8176 |
| Random Forest       |     0.8539 |      0.875  |   0.7206 | 0.7903 | 0.8597 |
| Tuned Random Forest |     0.8427 |      0.8571 |   0.7059 | 0.7742 | 0.8632 |

## Imbalance comparison
| Strategy                |   Precision |   Recall |     F1 |
|:------------------------|------------:|---------:|-------:|
| Baseline                |      0.7612 |   0.75   | 0.7556 |
| class_weight='balanced' |      0.726  |   0.7794 | 0.7518 |
| SMOTE                   |      0.7361 |   0.7794 | 0.7571 |

## Random Forest GridSearchCV
- Best parameters: {'model__max_depth': 10, 'model__max_features': 'sqrt', 'model__n_estimators': 300}
- Best CV F1: 0.7557
- OOB score: 0.8158

## Regression
| Regression Model   |     MAE |    RMSE |     R2 |   Adjusted R2 |
|:-------------------|--------:|--------:|-------:|--------------:|
| Linear Regression  | 16.4415 | 38.5912 | 0.2319 |        0.1449 |

## Combined model comparison
|    | Model Type     | Model               |   Accuracy |   Precision |   Recall |       F1 |      AUC |      MAE |     RMSE |       R2 |   Adjusted R2 |
|---:|:---------------|:--------------------|-----------:|------------:|---------:|---------:|---------:|---------:|---------:|---------:|--------------:|
|  0 | Classification | Logistic Regression |     0.8146 |      0.7612 |   0.75   |   0.7556 |   0.8609 | nan      | nan      | nan      |      nan      |
|  1 | Classification | Decision Tree       |     0.7921 |      0.8039 |   0.6029 |   0.6891 |   0.8176 | nan      | nan      | nan      |      nan      |
|  2 | Classification | Random Forest       |     0.8539 |      0.875  |   0.7206 |   0.7903 |   0.8597 | nan      | nan      | nan      |      nan      |
|  3 | Classification | Tuned Random Forest |     0.8427 |      0.8571 |   0.7059 |   0.7742 |   0.8632 | nan      | nan      | nan      |      nan      |
|  4 | Regression     | Linear Regression   |   nan      |    nan      | nan      | nan      | nan      |  16.4415 |  38.5912 |   0.2319 |        0.1449 |

## Heteroscedasticity


## Final recommendation
Deploy **Random Forest** because it has the highest held-out F1 score (0.7903) among the evaluated classification models and an AUC of 0.8597. F1 is useful here because it balances precision and recall rather than emphasizing only one. The regression results are presented separately because MAE, RMSE, R², and Adjusted R² are not directly comparable with classification metrics. The saved artifact is the complete preprocessing-plus-estimator pipeline and can accept raw input rows with the expected feature columns.
