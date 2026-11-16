# 5. Viva preparation (HA-4)

Say the one-line answer first, then add detail if asked.

## A. About the assignment

**Q. What was the aim?** To predict students' final exam marks from attendance, study hours, assignment marks and internal marks using simple linear regression, and to evaluate the model.
**Q. Dataset?** 100 students; 4 predictors and the target Final_Exam_Marks. No missing values or duplicates.
**Q. Final result?** Final = 46.42 + 0.364 × Internal_Marks. Test MAE 6.57, RMSE 7.97, R² 0.21.
**Q. Is this supervised or unsupervised? Regression or classification?** Supervised (we know the target), regression (target is numeric).

## B. Preprocessing and EDA

**Q. What preprocessing did you do?** Checked missing values and duplicates (none), dropped Student_ID, confirmed all columns numeric.
**Q. Why drop Student_ID?** It's only a label; it carries no information about performance and could mislead the model.
**Q. Is scaling needed?** Not for simple linear regression. Predictions don't change with scaling, only the coefficient units do.
**Q. What did EDA show?** Final marks are roughly bell shaped (mean 71, sd 10.6); no big outliers; scatter plots show Internal_Marks has the clearest upward trend.
**Q. How would you handle missing values or outliers if they existed?** Missing: drop rows or fill with mean/median. Outliers: check if they are errors, then remove, cap or keep (linear regression is sensitive to outliers).

## C. Correlation

**Q. What is correlation?** The strength and direction of the linear relationship between two variables.
**Q. Range of r?** −1 to +1. 0 = no linear relationship.
**Q. Strongest predictor?** Internal_Marks r = 0.599. Then Assignment 0.394, Study hours 0.368, Attendance 0.317.
**Q. What does r = 0.6 mean?** Moderate positive correlation; r² = 0.36 so about 36% of the variation in final marks is shared with internal marks.
**Q. Correlation vs covariance?** Covariance depends on units and has no fixed range; correlation is standardised (covariance divided by both standard deviations).
**Q. Does correlation mean causation?** No. Internal and final marks may both reflect ability or effort.
**Q. Can r = 0 still mean a relationship?** Yes, a non-linear one (e.g. a U shape).
**Q. Is r = 0.32 (attendance) significant?** Yes, p = 0.0013 < 0.05, so it's unlikely to be zero in the population, though it's a weak-to-moderate relationship.
**Q. What does the heatmap tell you?** Pairwise correlations at a glance: red positive, blue negative. Predictors are barely correlated with each other.
**Q. What is multicollinearity?** Predictors strongly correlated with each other. Not a problem here (|r| ≤ 0.12).

## D. Regression

**Q. What is simple linear regression?** Fitting a straight line y = b0 + b1x to predict y from one variable x.
**Q. How is the line found?** Least squares: minimise the sum of squared residuals. b1 = Cov(X,Y)/Var(X), b0 = ȳ − b1·x̄.
**Q. Why squares, not absolute values?** Errors don't cancel, big errors are punished more, and there is a simple formula. Absolute error would need iterative methods.
**Q. Interpret the slope 0.364.** One extra internal mark goes with about 0.364 extra final marks.
**Q. Interpret the intercept 46.42.** Predicted final marks at 0 internal marks; outside the data range (35 to 100), so it is only the starting point of the line.
**Q. What is a residual?** Actual minus predicted.
**Q. Assumptions of linear regression?** Linearity, independence of errors, normality of errors, equal variance of errors (homoscedasticity).
**Q. How did you check them?** Residuals vs predicted plot (no pattern, even spread), histogram and Q-Q plot (roughly normal), Shapiro-Wilk p = 0.90, mean residual ≈ 0.
**Q. Why Internal_Marks?** Highest correlation, and cross-validated R² is also highest (0.28 vs 0.07 to 0.11).
**Q. Why not use all four?** The question asks for *simple* linear regression. Multiple regression would be better (test R² 0.74) and I showed it as a comparison.
**Q. Can the model predict internal = 120 or 0?** No, that's extrapolation outside the data range (35 to 100); the line may not hold there.
**Q. Which library fits the model?** scikit-learn `LinearRegression`; I verified the slope and intercept by hand using the covariance formula.
**Q. Why is X written with double brackets `df[['Internal_Marks']]`?** scikit-learn needs X as a 2-D table (rows x features); single brackets give a 1-D Series.

## E. Train-test split

**Q. Why split the data?** To test on students the model has not seen, otherwise we'd just measure memorisation.
**Q. Why 80/20?** A common convention: enough data to learn, some left to test. With only 100 students the test set is small (20).
**Q. What is `random_state`?** The seed for the shuffle; fixing it makes the split repeatable.
**Q. What is overfitting?** Model fits training data too closely and performs poorly on new data. Here train R² 0.38 vs test 0.21 is mostly from the tiny test set; a one-variable line can't really overfit.
**Q. What is cross-validation and why did you use it?** Split the data into 5 parts, train on 4 and test on 1, rotate, average. The test score from one 20-student split is noisy, CV is steadier (R² 0.277).
**Q. What is data leakage?** Test information leaking into training (e.g. fitting on all data). We fit only on the training set.

## F. Metrics

**Q. Explain MAE, MSE, RMSE, R².**
- MAE = average |error|, in marks (6.57).
- MSE = average squared error, in marks² (63.57), punishes large errors.
- RMSE = √MSE, typical error in marks (7.97).
- R² = 1 − SSE/SST, fraction of variance explained (0.207).
**Q. Which metric is easiest to explain to a non-technical person?** MAE ("on average off by 6.6 marks").
**Q. Why RMSE > MAE?** Squaring emphasises large errors, so RMSE ≥ MAE always; the gap shows how uneven the errors are.
**Q. What does R² = 0.21 mean? Is that bad?** 21% of the variation in the test marks is explained by internal marks. It is modest, but exam marks depend on many things not in the data, and predicting just the average would be worse (RMSE 9.0 vs 7.97).
**Q. Can R² be negative?** Yes on test data, if the model is worse than predicting the mean.
**Q. Why is train R² higher than test R²?** The model is fitted to the training data, so it fits it best; plus the test set is small and noisy.
**Q. Does a high R² mean the model is good?** Not always (overfitting, wrong model form); check residuals and test data.
**Q. R² vs correlation?** For simple regression R² = r², e.g. 0.599² ≈ 0.359 on the full data.

## G. Tricky "why" questions

**Q. The test R² is only 0.21. Would you deploy this?** Not for exact predictions, only as a rough early-warning indicator. A multiple regression or more data would do better.
**Q. Why does attendance, which is usually important, have the weakest link?** In this dataset it correlates only r = 0.32 with marks. Data can disagree with intuition; it could also depend on how attendance is recorded.
**Q. If you ran again with a different `random_state`, would results change?** Yes, the test metrics would shift because they depend on which 20 students are in the test set; that is why I added cross-validation.
**Q. Could we use a non-linear model?** Possibly (polynomial, trees), but the scatter plot and residual plot show no curvature, so a line is adequate.
**Q. What would you do to improve?** Use all predictors (multiple regression), collect more data, add other features (previous grades), try regularisation or non-linear models.
**Q. Is the relationship causal?** No, only association.

## H. Formula sheet

| Item | Formula |
|---|---|
| Covariance | Σ(x − x̄)(y − ȳ) / (n − 1) |
| Pearson r | Cov(X,Y) / (s_x · s_y) |
| Slope | b1 = Cov(X,Y) / Var(X) = r · s_y / s_x |
| Intercept | b0 = ȳ − b1 · x̄ |
| Prediction | ŷ = b0 + b1 · x |
| Residual | e = y − ŷ |
| SSE / SST | Σ(y − ŷ)² / Σ(y − ȳ)² |
| MAE | (1/n) Σ\|y − ŷ\| |
| MSE | (1/n) Σ(y − ŷ)² |
| RMSE | √MSE |
| R² | 1 − SSE / SST (= r² for simple regression) |
| t for r | r √(n−2) / √(1 − r²) |

**Numbers to remember:** equation 46.42 + 0.364x; r = 0.599; test MAE 6.57, RMSE 7.97, R² 0.207; train R² 0.380; CV R² 0.277; multiple regression test R² 0.736.
