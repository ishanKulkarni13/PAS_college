# 10. Notebook Walkthrough

`property_location_rental_price_analysis.ipynb`: 93 cells (M = markdown, C = code). Cell numbers start at 0. Follow this order when presenting.

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
| 35-38 | M, C | Skewness, kurtosis, log(Rent), side-by-side histogram, comment | Report 3.2 (new) |
| 39-42 | M, C | City-wise count, mean, median, SD; bar chart; comment | Location analysis |
| 43-45 | M, C | Box plot by city (log axis) and comment | Report 3.3 (new) |
| 46-52 | M, C | Pairwise covariance and Pearson (47), correlation heatmap (49), scatter Size vs Rent (51), comments | Required covariance/correlation |
| 53-55 | M, C | Spearman vs Pearson table and comment | Report 5.2 (new) |
| 56-62 | M, C | LLN: reference mean (57), nested random samples (59), line plot (61), comments | Seminar topic |
| 63-66 | M, C | CLT: 1,000 sample means for n = 30 and n = 100 (64), histograms (65), comment | Supports inference (new) |
| 67-71 | M, C | Inference intro (alpha 0.05); 95% CI overall (69) and by city (70); comment | Report 4.1 (new) |
| 72-74 | M, C, M | Independent two-sample t-test (Student's, `equal_var=True`), Furnished vs Unfurnished: group summary, pooled SD, t by hand and by scipy, p-value, assumptions | Report 4.2-4.4 (new) |
| 75-77 | M, C, M | One-way ANOVA across six cities (`f_oneway`), interpretation and assumptions | (new) |
| 78-79 | M, C | Regression intro and metric definitions; train/test split; `scores()` function | Report 6 (new) |
| 80-85 | M, C | Simple regression: fit by formula (81), fitted line (82), residuals (83), effect of the Rs. 35 lakh row (84), interpretation (85) | Report 6.1 |
| 86-90 | M, C | Multiple regression on Rent and log(Rent): coefficients (87), model comparison and adjusted R-squared (88), plots (89), interpretation (90) | Report 6.2-6.4 |
| 91 | M | Key findings (8 points) | Report 7.2 |
| 92 | M | Conclusion, limitations and future scope | Report 7.4, 8.1, 8.2 |

## Key variables in the code (so you can explain any line)

| Name | Meaning |
|---|---|
| `df` | The whole dataset |
| `rent` | `df["Rent"]` |
| `cities` | City names sorted by mean rent, highest first (cell 41) |
| `numeric_data` | Rent, Size, BHK, Bathroom (complete rows) (cell 47) |
| `pairwise_results` | Table of the 6 pairs with covariance, Pearson, interpretation (cell 47) |
| `reference_mean` | Mean of all rents (cell 57) |
| `random_rents` | Rents in one random order (cell 59) |
| `clt_means` | Dict: n -> 1,000 sample means (cell 64) |
| `train_idx`, `test_idx` | Row positions of the training and test sets (cell 79) |
| `scores(actual, predicted)` | Returns MAE, MSE, RMSE, R-squared (cell 79) |
| `slope`, `intercept` | Simple regression coefficients (cell 81) |
| `X` | Design matrix: column of ones, Size, BHK, Bathroom, 5 city dummies (cell 87) |
| `beta_raw`, `beta_log` | Coefficients of the multiple models (cell 87) |

## Things you may be asked about in the code

- **`rent.var()` / `rent.std()`** use n-1 by default (sample formula).
- **`rent.sample(frac=1, random_state=42)`** shuffles all rows; the `expanding().mean()` gives the mean of the first n values for every n.
- **`equal_var=True`** in `stats.ttest_ind` makes it the traditional (pooled-variance) Student's t-test: it assumes equal population variances. The notebook also computes the pooled SD and the t statistic by hand and checks they match scipy.
- **`stats.f_oneway(*groups)`** takes one array per city (the `*` unpacks the list).
- **`np.linalg.lstsq(X, y)`** solves least squares: finds the coefficients that minimise the squared errors.
- **`pd.get_dummies(..., columns=["City"])` then dropping `City_Kolkata`** makes 0/1 columns with Kolkata as the baseline.
- **`np.exp(X @ beta_log)`** converts log-predictions back to rupees (`@` is matrix multiplication).

## Re-running

Run from the `cp` folder so that `data/House_Rent_Dataset.csv` is found. Restart kernel and Run All; the outputs will be identical because every random step uses a fixed seed. Run time is a few seconds.
