# 4. Results and how to say them in words

All values are from the executed `ha-4.ipynb`.

## Data
100 students, 5 numeric columns after dropping `Student_ID`; no missing values or duplicates.

| Variable | Mean | Std | Min | Max |
|---|---|---|---|---|
| Attendance_% | 77.38 | 13.46 | 55 | 100 |
| Study_Hours | 4.27 | 2.15 | 1.1 | 7.8 |
| Assignment_Marks | 74.34 | 16.83 | 42 | 99 |
| Internal_Marks | 66.87 | 18.71 | 35 | 100 |
| Final_Exam_Marks | 71.14 | 10.57 | 49 | 96 |

## Correlation with Final_Exam_Marks
| Predictor | r | p-value | Strength (slide 62) |
|---|---|---|---|
| Internal_Marks | 0.599 | 4.6e-11 | moderate (strongest) |
| Assignment_Marks | 0.394 | 4.9e-05 | moderate |
| Study_Hours | 0.368 | 1.7e-04 | moderate |
| Attendance_% | 0.317 | 1.3e-03 | moderate (lowest) |

Between the predictors: all correlations are within ±0.12, so they carry mostly separate information.

**Say it like this:** "All four factors are positively correlated with the final marks, and all are statistically significant. Internal marks has the strongest relationship (r = 0.60). Even so, r² is only 0.36, so internal marks explains about 36% of the variation in the final marks."

## The model
Train on 80 students, test on 20 (`random_state = 42`):

**Final_Exam_Marks = 46.42 + 0.364 × Internal_Marks**

- Slope 0.364 (standard error 0.053, p ≈ 1.2e-9 on the training data): each extra internal mark goes with about 0.36 extra final marks. 10 more internal marks means about 3.6 more final marks.
- Intercept 46.42: predicted marks if internal marks were 0. Outside the data range (35 to 100), so just an anchor.
- Predictions: internal 45 → 62.8, 65 → 70.1, 85 → 77.4.

## Metrics
| | MAE | MSE | RMSE | R² |
|---|---|---|---|---|
| Training (80) | 6.84 | 73.25 | 8.56 | 0.380 |
| Testing (20) | 6.57 | 63.57 | 7.97 | 0.207 |

**Say it like this:**
- MAE 6.57: "On unseen students the prediction is off by about 6.6 marks on average."
- RMSE 7.97: "A typical error is around 8 marks; it is larger than MAE because big misses count more."
- R² 0.207: "The model explains about 21% of the variation in test marks; the other 79% comes from things not in the model."
- For context: always guessing the average marks gives a test RMSE of about 9.0, so the model (7.97) is an improvement, but a modest one.

**Why is test R² (0.21) lower than train R² (0.38)?** Mostly because the test set is only 20 students, so its score is noisy. The 5-fold cross-validated R² is 0.277 and the R² on all 100 students is about 0.36, which tell the same story. It is not severe overfitting; a straight line has just two parameters.

## Residual checks
- Residuals vs predicted: random scatter around 0, no curve, roughly even spread → linearity and constant variance look fine.
- Histogram: roughly bell shaped. Q-Q plot: points close to the line. Shapiro-Wilk p = 0.90 (> 0.05) → normal errors plausible.
- Mean residual on all data 0.36 (≈ 0).
- Independence: each student is a separate record, so assumed.

## Other predictors and multiple regression
| Predictor | Test R² | CV R² |
|---|---|---|
| Internal_Marks | 0.207 | 0.277 |
| Assignment_Marks | 0.154 | 0.111 |
| Study_Hours | 0.048 | 0.099 |
| Attendance_% | 0.003 | 0.065 |

Multiple regression (all four): test MAE 3.68, RMSE 4.60, R² 0.736. Fitted on the training data, the equation is Final = −0.73 + 0.262·Attendance + 2.269·Study_Hours + 0.247·Assignment_Marks + 0.358·Internal_Marks. (The notebook only prints the metrics for this one; these coefficients were checked separately.)

**Say it like this:** "Internal marks is the best single predictor, but one factor explains only a fifth of the variation. Using all four together reduces the typical error from about 8 marks to about 4.6 and raises R² from 0.21 to 0.74, because each factor adds separate information."

## Final conclusion in one paragraph (memorise)
"Student data for 100 students was cleaned and explored. All four factors are positively and significantly correlated with the final marks, with internal marks being the strongest (r = 0.60). A simple linear regression on internal marks gives Final = 46.42 + 0.364 × Internal. On unseen students the MAE is about 6.6 marks, RMSE about 8 and R² about 0.21 (0.28 in cross-validation). The residuals support the linear model assumptions. The model is only a rough indicator because one factor cannot explain most of the variation; using all four factors would give much better predictions (R² ≈ 0.74). The results show association, not causation."

## Limitations to mention
- Only 100 students; test set of 20 is small, so metric values change with the random split.
- Only a linear relationship with one predictor was modelled.
- Marks depend on things not in the data (prior knowledge, health, difficulty of paper).
- Correlation is not causation.
