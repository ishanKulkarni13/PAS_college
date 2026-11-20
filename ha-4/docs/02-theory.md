# 2. Theory and intuition (HA-4)

## 2.1 The machine learning picture
- **Supervised learning**: we have inputs (X) with known outputs (y) and learn a rule that maps X to y.
- **Regression**: y is a number (marks). **Classification**: y is a category (pass/fail).
- **Training data** is used to learn the rule; **test data** (unseen) is used to measure how well the rule generalises.
- **Overfitting**: model memorises training data and does badly on new data. **Underfitting**: model is too simple to capture the pattern. A big gap between train and test performance warns about overfitting.

## 2.2 Preprocessing and EDA
- **Missing values / duplicates**: remove or fix; here there are none.
- **ID columns** carry no meaning for prediction; leave them out (a model could otherwise "learn" the ID order).
- **Categorical data** would need encoding; all our columns are numeric.
- **Scaling** matters for distance based models (KNN, SVM) and for regularised models. Plain linear regression gives the same predictions with or without scaling, only the coefficients change units.
- **EDA**: histograms show the shape/spread of each variable; scatter plots show the relationship between two variables; describe() gives mean, std, quartiles. Look for outliers and weird values.

## 2.3 Correlation (Unit I slide 62 for the strength table)
- **Covariance**: Cov(X,Y) = Σ(x−x̄)(y−ȳ)/(n−1). Positive when X and Y move together. Its size depends on units, so it is hard to compare.
- **Pearson correlation**: r = Cov(X,Y)/(s_x s_y), always between −1 and +1.
  - r > 0: positive (both rise together). r < 0: negative. r ≈ 0: no *linear* relationship.
  - Strength (slide 62): 0.7 to 0.99 strong, 0.3 to 0.69 moderate, below 0.3 weak (and the same for negative).
- r measures **linear** association only. A perfect curve (like y = x²) can have r ≈ 0.
- **Correlation ≠ causation.** Internal marks relating to final marks does not prove internal marks cause final marks (both may reflect ability or effort).
- **Significance of r**: test H0: ρ = 0 with t = r√(n−2)/√(1−r²). Small p means the correlation is unlikely to be just chance (`scipy.stats.pearsonr` gives r and p).
- **r² (coefficient of determination)** for a single predictor = share of variance in y explained by x.
- **Multicollinearity**: when predictors are strongly correlated with each other. It matters for multiple regression (coefficients become unstable). Ours are barely correlated (|r| ≤ 0.12).

## 2.4 Simple linear regression
Model: **y = β0 + β1·x + ε**
- β0 = intercept (predicted y when x = 0), β1 = slope (change in y for one unit change in x), ε = random error.
- Fitted line: ŷ = b0 + b1·x.
- **Residual** e_i = y_i − ŷ_i (actual minus predicted).
- **Least squares**: choose b0, b1 that minimise **SSE = Σ(y_i − ŷ_i)²**.
  - **b1 = Cov(X,Y)/Var(X) = r · (s_y/s_x)**
  - **b0 = ȳ − b1·x̄** (so the line always passes through the point (x̄, ȳ))
- Why squared errors? Positive and negative errors don't cancel, large errors are punished more, and the maths gives a neat closed form solution (no iteration needed).
- scikit-learn's `LinearRegression().fit(X, y)` does exactly this. X must be 2-D (`df[['Internal_Marks']]`) and y 1-D.
- **Interpreting the slope**: "for each extra internal mark, predicted final marks rise by b1". **Intercept**: only meaningful if x = 0 is realistic; here internal marks never go below 35, so it is just an anchor of the line.
- **Significance of the slope**: t-test of H0: β1 = 0; for simple regression it is equivalent to testing correlation (same p).

### Assumptions (LINE)
1. **L**inearity: the relationship is a straight line (residuals vs fitted: no curve).
2. **I**ndependence of errors (students are separate records).
3. **N**ormality of errors (histogram / Q-Q plot / Shapiro-Wilk of residuals).
4. **E**qual variance of errors (homoscedasticity; residual spread is the same across fitted values).
Also: the model should not be used outside the range of x seen in the data (extrapolation).

## 2.5 Evaluation metrics
Let n = number of observations, y = actual, ŷ = predicted.

| Metric | Formula | Meaning | Good when |
|---|---|---|---|
| **MAE** | (1/n) Σ\|y − ŷ\| | average absolute error, in marks | low |
| **MSE** | (1/n) Σ(y − ŷ)² | average squared error (marks²), punishes big errors | low |
| **RMSE** | √MSE | typical error, back in marks; ≥ MAE | low |
| **R²** | 1 − SSE/SST, SST = Σ(y − ȳ)² | fraction of variance explained | close to 1 |

- **R² intuition**: compare our model's error to the dumbest model (always predicting the mean ȳ). R² = 0 means no better than the mean; R² = 1 is perfect; R² can even be negative on test data if the model is worse than predicting the mean.
- RMSE ≥ MAE always; a large gap means a few big errors.
- Compare train vs test metrics: much worse on test = overfitting; here the test set is just 20 students so it is noisy.
- **Adjusted R²** penalises extra predictors (only relevant for multiple regression).
- A "good" R² depends on the field. Human performance data typically gives modest R² because many unmeasured things (health, luck, question paper difficulty) affect marks.

## 2.6 Train-test split and cross-validation
- `train_test_split(X, y, test_size=0.2, random_state=42)`: 80 students to learn, 20 to test. `random_state` fixes the shuffle so results can be repeated. Another seed gives different numbers.
- With a small dataset the test score depends heavily on which 20 students were chosen. **k-fold cross-validation** splits into k parts (we used 5), trains on 4 and tests on the 1 left out, rotates, and averages the scores. It is a steadier estimate than one split.
- **Data leakage**: information from the test set must not influence training. We fit only on the training data.

## 2.7 Simple vs multiple regression
- Simple: one predictor. Multiple: ŷ = b0 + b1x1 + b2x2 + …; each coefficient is the effect of that variable *holding the others fixed*.
- With more informative predictors the model explains more. Here the four factors together give a test R² of 0.74 versus 0.21 for internal marks alone, because each factor adds separate information (they are not correlated with each other).
- The assignment asks for the simple version, which is why the model uses one variable and the multiple version is only shown as a comparison.

## 2.8 Library summary
| Item | Library / function |
|---|---|
| Data | `pandas.read_csv`, `df.describe()`, `df.corr()` |
| Plots | `matplotlib` (hist, scatter, imshow for heatmap) |
| Correlation + p-value | `scipy.stats.pearsonr` |
| Split | `sklearn.model_selection.train_test_split` |
| Model | `sklearn.linear_model.LinearRegression` |
| Metrics | `sklearn.metrics.mean_absolute_error`, `mean_squared_error`, `r2_score` |
| Cross-validation | `sklearn.model_selection.cross_val_score`, `KFold` |
