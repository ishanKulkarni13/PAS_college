# 02. Report Template Coverage

Maps every chapter/section of `cp/reference/PAS Course Project Report template.docx` to the **cell numbers of the notebook** (`property_location_rental_price_analysis.ipynb`, 0-indexed, 96 cells; the number is the position of the cell in the notebook, counting from 0 at the top).

Legend: **Done** = implemented and interpreted in the notebook. **Text only** = no code needed, content goes into the report from the docs. **Report** = only exists in the .docx.

## Coverage table

| Report section | Notebook cells | What those cells cover | Status | Doc |
|---|---|---|---|---|
| **Front matter** (title page, certificate, synopsis, acknowledgement, abstract, lists of figures/tables) | none | Written in the report. Placeholders are listed in `report/NOTES.md` | Report | [report/NOTES.md](report/NOTES.md) |
| **1.1 Overview of the Project** | 0, 1 | Title, group seminar topic, dataset link, objectives | Text only | [01](01-project-overview.md) |
| **1.2 Problem Statement** | 1 | Objectives; problem statement paragraph is in doc 01 | Text only | [01](01-project-overview.md) |
| **1.3 Objective** | 1 | Bullet list of 7 objectives | Done | [01](01-project-overview.md) |
| **1.4 Scope of the Project** | 1, 95 | Objectives, limitations, "what we did not do" | Text only | [01](01-project-overview.md), [12](12-limitations-and-future-scope.md) |
| **2.1 Dataset Features and Variables** | 3, 4, 5, 8, 9, 10, 11, 18, 19 | Loading (3), first rows (4), column types and non-null counts (5), city counts (8), rent/size ranges (9), variable-type table with "used in analysis" (11), describe table (18), furnishing counts (19) | Done | [03](03-dataset-sampling-cleaning.md) |
| **2.2 Sampling Method and Sample Size** | 12, 13, 56-62, 63-66 | Date window, area type, contact (12); sampling method, sample size n=4,746 and bias note (13); random sampling for LLN with fixed seed and nested samples (56-62); sampling distribution of the mean for n=30 and n=100 (63-66) | Done | [03](03-dataset-sampling-cleaning.md), [05](05-lln-and-clt.md) |
| **2.3 Data Cleaning and Preprocessing** | 6, 7, 14, 15, 16, 32, 82 | Missing values (6), duplicates (7), cleaning note (14), non-positive check (15), very small sizes (16), IQR outlier check (32), train/test split (82) | Done | [03](03-dataset-sampling-cleaning.md) |
| **2.4 Sampling Bias and Limitations** | 13, 95 | Convenience-sample bias (13); limitations list (95) | Done | [03](03-dataset-sampling-cleaning.md), [12](12-limitations-and-future-scope.md) |
| **3.1 Descriptive Statistics** (mean, median, SD, IQR) | 18, 24-31, 33, 34, 40 | Describe table (18), mean (24), median (25), mode (26), comment (27), variance (28), SD (29), quartiles and IQR (30), min/max/range (31), summary table of Rent/Size/BHK/Bathroom (34), city-wise count/mean/median/SD (40) | Done | [04](04-descriptive-and-distribution.md) |
| **3.2 Data Distribution Analysis** | 21, 22, 32, 35, 36, 37, 38 | Histogram and comment (21-22), IQR outliers (32), skewness and kurtosis of Rent and log(Rent) (36), side-by-side histograms (37), interpretation (38) | Done | [04](04-descriptive-and-distribution.md) |
| **3.3 Data Visualization** (histogram, box plot, scatter, etc.) | 21, 37, 41, 44, 49, 51, 61, 65, 85, 86, 92 | Histogram (21), Rent vs log(Rent) histograms (37), bar chart of mean rent by city (41), **box plot by city** (44), correlation heatmap (49), **scatter Size vs Rent** (51), LLN line plot (61), CLT histograms (65), regression line (85), residual plot (86), log-model plots (92) | Done | [04](04-descriptive-and-distribution.md) |
| **3.4 Observations and Interpretation** | 22, 27, 38, 42, 45, 48, 50, 52, 55 | The markdown comment under each result | Done | 04, 07 |
| **4.1 Confidence Interval Estimation (95%)** | 68, 69, 70, 71 | Formula (68), overall CI with check against scipy (69), CI per city (70), interpretation (71) | Done | [06](06-inference.md) |
| **4.2 Hypothesis Formulation (H0 and H1)** | 67, 72, 75, 78 | alpha = 0.05 (67); H0/H1 for the Mumbai test (72), furnishing test (75), ANOVA (78) | Done | [06](06-inference.md) |
| **4.3 Hypothesis Testing (Z-test / t-test / ANOVA)** | 73, 76, 79 | Welch t-test Mumbai vs others (73), Welch t-test Furnished vs Unfurnished (76), one-way ANOVA plus Kruskal-Wallis plus Levene (79) | Done | [06](06-inference.md) |
| **4.4 Statistical Significance and Interpretation** | 74, 77, 80 | Decision and meaning of p-values for each test | Done | [06](06-inference.md) |
| **5.1 Selection of Variables** | 11, 46, 47 | Variable-type table (11); the four numerical variables chosen and why (46-47) | Done | [07](07-correlation.md) |
| **5.2 Pearson or Spearman Correlation** | 47, 49, 51, 53, 54 | Pairwise covariance and Pearson with matrices (47), heatmap (49), scatter (51), why Spearman (53), Pearson vs Spearman table (54) | Done | [07](07-correlation.md) |
| **5.3 Interpretation of Correlation Results** | 48, 50, 52, 55 | Strength bands, heatmap reading, scatter reading, Spearman vs Pearson | Done | [07](07-correlation.md) |
| **6.1 Simple Linear Regression (mandatory)** | 81, 82, 83, 84, 85, 86, 87, 88 | Metric definitions (81), train/test split and scores function (82), method (83), fit, equation, scipy check, train/test metrics (84), fitted line (85), residual plot (86), effect of the Rs. 35 lakh listing (87), interpretation (88) | Done | [08](08-regression.md) |
| **6.2 Multiple Linear Regression (optional)** | 89, 90, 91, 92, 93 | Design and dummy variables (89), coefficients for Rent and log(Rent) (90), comparison of the 3 models and adjusted R-squared (91), plots (92), interpretation (93) | Done | [08](08-regression.md) |
| **6.3 Regression Equation and Coefficients** | 84, 90 | Simple equation (84); coefficient table for both multiple models with % effects (90) | Done | [08](08-regression.md) |
| **6.4 Regression Plots and Interpretation** | 85, 86, 87, 88, 92, 93 | Fitted line, residuals, actual vs predicted, interpretation | Done | [08](08-regression.md) |
| **7.1 Summary of Statistical Results** | 34, 40, 47, 54, 59, 69, 73, 76, 79, 84, 91, 94 | All result tables; one-page summary in doc 09 | Done | [09](09-results-summary.md) |
| **7.2 Key Findings and Observations** | 94 | Eight numbered findings | Done | [09](09-results-summary.md) |
| **7.3 Practical Applications and Implications** | none | Not code. Text for the report is in doc 12 | Text only | [12](12-limitations-and-future-scope.md) |
| **7.4 Limitations of the Study** | 95 | Limitations list | Done | [12](12-limitations-and-future-scope.md) |
| **8.1 Conclusion** | 95 | Conclusion paragraphs | Done | [09](09-results-summary.md) |
| **8.2 Future Scope** | 95 | Future scope list | Done | [12](12-limitations-and-future-scope.md) |
| **References** | none | Textbooks from the syllabus, scipy/pandas/matplotlib, Kaggle dataset | Report | [12](12-limitations-and-future-scope.md) |

## Extra content that is not in the template

| Notebook cells | Content | Why it is there |
|---|---|---|
| 56-62 | Law of Large Numbers demonstration | Group seminar topic |
| 63-66 | Central Limit Theorem demonstration | Justifies using t-tests and CIs on skewed data |

## Template-to-syllabus check

| Syllabus topic | Where it is covered |
|---|---|
| Types of data and variables | cell 11 |
| Sampling techniques, sampling bias | cell 13 (convenience sample) |
| Data cleaning and preprocessing | cells 6-7, 14-16 |
| EDA | cells 17-45 |
| Measures of central tendency and dispersion | cells 24-34 |
| Covariance and correlation (Pearson and Spearman) | cells 46-55 |
| Sampling distribution and Central Limit Theorem | cells 63-66 |
| Estimation and confidence intervals | cells 68-71 |
| Hypothesis formulation, independent t-test, one-way ANOVA | cells 72-80 |
| Simple and multiple linear regression, regression assumptions | cells 81-93 |
| MAE, MSE, RMSE, R-squared | cells 82-84, 91 |
| End-to-end workflow | whole notebook |
| Not covered (not applicable to this dataset) | One-sample Z-test, paired t-test (a paired design needs before/after data), probability distributions (Binomial/Poisson) |

## Cells that existed before Phase II (unchanged)

0-9, 14-34 (except the new box/skewness cells listed above), 39-42, 46-52, 56-62. The imports cell (2) and the objectives cell (1) were extended, and the key findings (94) and conclusion (95) were rewritten to include the new work.

## Report-writing gaps (not code)

Sections 1.1, 1.2, 1.4, 7.3 and the front matter have to be written as report text. The raw material is in docs 01 and 12; placeholders (names, PRNs, guide) are in `report/NOTES.md`.
