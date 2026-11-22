# 10. Notebook Walkthrough

> **Phase III (branch `cp-phase-3`):** the notebook now has 89 cells (it had 92). The three Spearman cells were deleted, the correlation and LLN code was simplified, and the later cells moved up by 3. The table and variable list below use the new numbers. The old-to-new cell map is in [13-phase-3-changes-from-main.md](13-phase-3-changes-from-main.md).

`property_location_rental_price_analysis.ipynb`: 89 cells (M = markdown, C = code). Cell numbers start at 0. Follow this order when presenting.

| Cells | Type | What happens | Why |
|---|---|---|---|
| 0-1 | M | Title, dataset link, 7 objectives | Context |
| 2 | C | Imports: pandas, numpy, matplotlib, scipy.stats | Only four libraries are needed |
| 3-5 | C | Load CSV, `head()`, `info()` | See shape (4746, 12), types, no nulls |
| 6-7 | C | Missing values and duplicates | Data quality: none |
| 8-9 | C | City counts, rent and size ranges | Know the groups and extremes |
| 10-13 | M, C | Variable-type table; date window, area type, contact; sampling method and bias note | Report 2.1 and 2.2 |
| 14-16 | M, C | Cleaning: rent/size positive, 16 tiny sizes shown | Report 2.3 |
| 17-19 | M, C | `describe()` and furnishing counts | First look |
| 20-22 | M, C, M | Histogram of Rent and comment (right-skewed) | Distribution shape |
| 23-32 | M, C | Mean, median, mode (24-26), comment (27), variance (28), SD (29), Q1/Q3/IQR (30), min/max/range (31), IQR outlier rule (32) | Required measures |
| 33-34 | M, C | Summary table for Rent, Size, BHK, Bathroom | Compact descriptive table |
| 35-38 | M, C | Skewness, kurtosis, histogram, comment | Report 3.2 (new) |
| 39-42 | M, C | City-wise count, mean, median, SD; bar chart; comment | Location analysis |
| 43-45 | M, C | Box plot by city (log axis) and comment | Report 3.3 (new) |
| 46-52 | M, C | Covariance matrix and Pearson correlation matrix (47), correlation heatmap (49), scatter Size vs Rent (51), short comments | Required covariance/correlation |
| 53-59 | M, C | LLN: reference mean (54), nested random samples (56), line plot (58), comments | Seminar topic |
| 60-63 | M, C | CLT: 1,000 sample means for n = 30 and n = 100 (61), histograms (62), comment | Supports inference (new) |
| 64-68 | M, C | Inference intro (alpha 0.05); 95% CI overall (66) and by city (67); comment | Report 4.1 (new) |
| 69-71 | M, C, M | Independent two-sample t-test (Student's, pooled variance), Furnished vs Unfurnished: group summary, pooled variance, SD and standard error, t by hand, p-value from the t distribution (`stats.t.sf`), assumptions | Report 4.2-4.4 (new) |
| 72-74 | M, C, M | One-way ANOVA across six cities (`f_oneway`), interpretation and assumptions | (new) |
| 75-76 | M, C | Regression intro and metric definitions; train/test split; `scores()` function | Report 6 (new) |
| 77-82 | M, C | Simple regression: fit by formula (78), fitted line (79), residuals (80), effect of the Rs. 35 lakh row (81), short interpretation (82) | Report 6.1 |
| 83-86 | M, C | Multiple regression on Rent: coefficients (84), model comparison and adjusted R-squared (85), interpretation (86) | Report 6.2-6.4 |
| 87 | M | Key findings (8 points) | Report 7.2 |
| 88 | M | Conclusion, limitations and future scope | Report 7.4, 8.1, 8.2 |

## Key variables in the code (so you can explain any line)

| Name | Meaning |
|---|---|
| `df` | The whole dataset |
| `rent` | `df["Rent"]` |
| `cities` | City names sorted by mean rent, highest first (cell 41) |
| `numeric_data` | Rent, Size, BHK, Bathroom (cell 47) |
| `covariance_matrix`, `correlation_matrix` | Covariance and Pearson correlation of those four columns (cell 47) |
| `reference_mean` | Mean of all rents (cell 54) |
| `random_rents` | Rents in one random order (cell 56) |
| `clt_means` | Dict: n -> 1,000 sample means (cell 61) |
| `train_idx`, `test_idx` | Row positions of the training and test sets (cell 76) |
| `scores(actual, predicted)` | Returns MAE, MSE, RMSE, R-squared (cell 76) |
| `slope`, `intercept` | Simple regression coefficients (cell 78) |
| `X` | Design matrix: column of ones, Size, BHK, Bathroom, 5 city dummies (cell 84) |
| `beta_raw` | Coefficients of the multiple model (cell 84) |

Variables that no longer exist after Phase III: `pairwise_results`, `compare_corr`, `spearman_matrix` (correlation), `valid_rents`, `random_state`, `requested_sample_sizes` (LLN).

## Things you may be asked about in the code

- **`rent.var()` / `rent.std()`** use n-1 by default (sample formula).
- **`rent.sample(frac=1, random_state=42)`** shuffles all rows; the `expanding().mean()` gives the mean of the first n values for every n.
- **`2 * stats.t.sf(abs(t_statistic), df_t)`** gives the two-tailed p-value from the Student's t distribution (`sf` is the upper-tail probability, 1 - cdf, but accurate for very small values). The pooled variance, pooled SD, standard error, t statistic and degrees of freedom are calculated by hand.
- **`stats.f_oneway(*groups)`** takes one array per city (the `*` unpacks the list).
- **`np.linalg.lstsq(X, y)`** solves least squares: finds the coefficients that minimise the squared errors.
- **`pd.get_dummies(..., columns=["City"])` then dropping `City_Kolkata`** makes 0/1 columns with Kolkata as the baseline.
- **`X[test_idx] @ beta_raw`** gives the predicted rents (`@` is matrix multiplication).

## Re-running

Run from the `cp` folder so that `data/House_Rent_Dataset.csv` is found. Restart kernel and Run All; the outputs will be identical because every random step uses a fixed seed. Run time is a few seconds.
