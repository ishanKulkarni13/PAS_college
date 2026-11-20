# 3. Code walkthrough: `ha-4.ipynb`

## Setup and loading
| Block | What it does | Why |
|---|---|---|
| imports | pandas, numpy, matplotlib, `scipy.stats`, and from scikit-learn: `train_test_split`, `cross_val_score`, `KFold`, `LinearRegression`, `mean_absolute_error`, `mean_squared_error`, `r2_score` | the ML pieces: split, model, metrics |
| `pd.read_csv`, `df.head()` | load and look at the first rows | sanity check |
| `df.shape`, `df.info()` | 100 rows x 6 columns, all numeric | no encoding needed |
| `isnull().sum()`, `duplicated().sum()` | both 0 | nothing to clean |
| `df.drop(columns='Student_ID')` | removes the ID | an ID is a label, not information |
| `df.describe()` | mean / std / quartiles per column | Final marks mean 71.14, std 10.57, range 49 to 96 |

## EDA
- Histogram grid: `plt.subplots(2, 3)`, loops over columns with `zip(axes.flat, df.columns)`; the 6th empty axis is switched off. Final marks look bell shaped; other columns are fairly flat/uniform.
- Scatter grid (2x2): each predictor vs `Final_Exam_Marks`. Internal_Marks shows the clearest upward trend; the others are weaker.

## Correlation
- `df.corr()`: full Pearson matrix. The last column/row shows each factor against Final marks: 0.317, 0.368, 0.394, 0.599.
- Heatmap: `plt.imshow(corr, cmap='coolwarm', vmin=-1, vmax=1)`, with `plt.text` writing the numbers in each cell. Red = positive, blue = negative, the diagonal is 1.
- `stats.pearsonr(x, y)` loop: gives r and the p-value for each predictor. All p < 0.05 (Internal_Marks p ≈ 4.6e-11), so every correlation is statistically significant.
- Decision: Internal_Marks has the highest r, so it becomes the single predictor.

## Split and model
- `X = df[['Internal_Marks']]` (double brackets keep it a 2-D table, which scikit-learn requires), `y = df['Final_Exam_Marks']`.
- `train_test_split(X, y, test_size=0.2, random_state=42)`: 80 train, 20 test.
- `LinearRegression().fit(X_train, y_train)`: learns b0 and b1 by least squares.
- `model.intercept_` = 46.4212, `model.coef_[0]` = 0.3642. Equation: **Final = 46.42 + 0.364 × Internal**.
- **Hand check**: `np.cov(x, y)[0,1] / np.var(x, ddof=1)` gives the same slope; `y.mean() − b1·x.mean()` gives the same intercept. This proves what `fit` does (b1 = Cov/Var, b0 = ȳ − b1·x̄).
- Example predictions: 45 → 62.8, 65 → 70.1, 85 → 77.4. `predict` takes a DataFrame with the same column name.

## Evaluation
- `model.predict(X_train)` and `model.predict(X_test)`.
- `get_metrics(actual, pred)`: my small function returning MAE, MSE, RMSE (`np.sqrt(mse)`), R² in a dict; dict-of-dicts goes into a DataFrame and `.T` flips it so rows are Training/Testing.

| | MAE | MSE | RMSE | R² |
|---|---|---|---|---|
| Training | 6.836 | 73.253 | 8.559 | 0.3795 |
| Testing | 6.570 | 63.570 | 7.973 | 0.2068 |

## Plots
1. Regression line: scatter of train (blue) and test (orange) points, red line from `model.predict` over a `np.linspace` of x values.
2. Actual vs predicted (test): points near the dashed y = x line are good predictions. Scatter around it shows the errors.
3. Table of 10 test students with Actual, Predicted, Error (e.g. student with actual 87 predicted 70.5, an error of 16.5).
4. Residual analysis (all 100 students): residual = actual − fitted.
   - residuals vs predicted: random cloud around 0 = linearity + constant variance OK
   - histogram: roughly bell shaped
   - `stats.probplot` Q-Q plot: points near the line = roughly normal
   - mean residual 0.36 (close to 0), Shapiro-Wilk p = 0.90 (> 0.05 so normal residuals are plausible)

## Extras
- Loop over all 4 predictors: for each, fit simple regression on the same train split, compute test metrics, plus `cross_val_score(..., cv=KFold(5, shuffle=True, random_state=42), scoring='r2').mean()`:

| Predictor | Slope | Test MAE | Test RMSE | Test R² | CV R² |
|---|---|---|---|---|---|
| Internal_Marks | 0.364 | 6.570 | 7.973 | 0.207 | 0.277 |
| Assignment_Marks | 0.251 | 6.186 | 8.232 | 0.154 | 0.111 |
| Study_Hours | 1.970 | 7.442 | 8.734 | 0.048 | 0.099 |
| Attendance_% | 0.277 | 6.890 | 8.937 | 0.003 | 0.065 |

  Internal_Marks wins on the cross-validated R², confirming the choice. (Assignment_Marks has a slightly lower test MAE on this one split, which shows how noisy a 20-student test set is.)
- Multiple regression with all four predictors: test MAE 3.675, RMSE 4.602, R² 0.736. Shown just for comparison.

## Notes in case Sir asks about code details
- `random_state=42`: any fixed number; makes the split repeatable.
- Why `fit` on train only: using test data in fitting would leak information and inflate the score.
- `LinearRegression` has no hyper-parameters worth tuning for this task; it uses the closed-form least squares solution.
- sklearn `r2_score` can be negative on test data.
