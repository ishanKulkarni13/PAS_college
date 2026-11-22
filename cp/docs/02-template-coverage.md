# 02. Report Template Coverage

> **Phase III (branch `cp-phase-3`):** this table was re-numbered for the new 89-cell notebook (the three Spearman cells were deleted, so every cell after the correlation section moved up by 3). Section 5.2 is now Pearson only, and the "equal variances not verified" remarks were removed. The old-to-new cell map is in [13-phase-3-changes-from-main.md](13-phase-3-changes-from-main.md).

Maps every chapter/section of `cp/reference/PAS Course Project Report template.docx` to the **cell numbers of the notebook** (`property_location_rental_price_analysis.ipynb`, 0-indexed, 89 cells; the number is the position of the cell in the notebook, counting from 0 at the top).

Legend: **Done** = implemented and interpreted in the notebook. **Text only** = no code needed, content goes into the report from the docs. **Report** = only exists in the .docx.

## Coverage table

| Report section | Notebook cells | What those cells cover | Status | Doc |
|---|---|---|---|---|
| **Front matter** (title page, certificate, synopsis, acknowledgement, abstract, lists of figures/tables) | none | Written in the report. Placeholders are listed in `report/NOTES.md` | Report | [report/NOTES.md](report/NOTES.md) |
| **1.1 Overview of the Project** | 0, 1 | Title, group seminar topic, dataset link, objectives | Text only | [01](01-project-overview.md) |
| **1.2 Problem Statement** | 1 | Objectives; problem statement paragraph is in doc 01 | Text only | [01](01-project-overview.md) |
| **1.3 Objective** | 1 | Bullet list of 7 objectives | Done | [01](01-project-overview.md) |
| **1.4 Scope of the Project** | 1, 88 | Objectives, limitations, "what we did not do" | Text only | [01](01-project-overview.md), [12](12-limitations-and-future-scope.md) |
| **2.1 Dataset Features and Variables** | 3, 4, 5, 8, 9, 10, 11, 18, 19 | Loading (3), first rows (4), column types and non-null counts (5), city counts (8), rent/size ranges (9), variable-type table with "used in analysis" (11), describe table (18), furnishing counts (19) | Done | [03](03-dataset-sampling-cleaning.md) |
| **2.2 Sampling Method and Sample Size** | 12, 13, 53-59, 60-63 | Date window, area type, contact (12); sampling method, sample size n=4,746 and bias note (13); random sampling for LLN with fixed seed and nested samples (53-59); sampling distribution of the mean for n=30 and n=100 (60-63) | Done | [03](03-dataset-sampling-cleaning.md), [05](05-lln-and-clt.md) |
| **2.3 Data Cleaning and Preprocessing** | 6, 7, 14, 15, 16, 32, 76 | Missing values (6), duplicates (7), cleaning note (14), non-positive check (15), very small sizes (16), IQR outlier check (32), train/test split (76) | Done | [03](03-dataset-sampling-cleaning.md) |
| **2.4 Sampling Bias and Limitations** | 13, 88 | Convenience-sample bias (13); limitations list (88) | Done | [03](03-dataset-sampling-cleaning.md), [12](12-limitations-and-future-scope.md) |
| **3.1 Descriptive Statistics** (mean, median, SD, IQR) | 18, 24-31, 33, 34, 40 | Describe table (18), mean (24), median (25), mode (26), comment (27), variance (28), SD (29), quartiles and IQR (30), min/max/range (31), summary table of Rent/Size/BHK/Bathroom (34), city-wise count/mean/median/SD (40) | Done | [04](04-descriptive-and-distribution.md) |
| **3.2 Data Distribution Analysis** | 21, 22, 32, 35, 36, 37, 38 | Histogram and comment (21-22), IQR outliers (32), skewness and kurtosis of Rent (36), histogram (37), interpretation (38) | Done | [04](04-descriptive-and-distribution.md) |
| **3.3 Data Visualization** (histogram, box plot, scatter, etc.) | 21, 37, 41, 44, 49, 51, 58, 62, 79, 80 | Histogram (21), histogram of Rent (37), bar chart of mean rent by city (41), **box plot by city** (44), correlation heatmap (49), **scatter Size vs Rent** (51), LLN line plot (58), CLT histograms (62), regression line (79), residual plot (80) | Done | [04](04-descriptive-and-distribution.md) |
| **3.4 Observations and Interpretation** | 22, 27, 38, 42, 45, 48, 50, 52 | The markdown comment under each result | Done | 04, 07 |
| **4.1 Confidence Interval Estimation (95%)** | 65, 66, 67, 68 | Formula (65), overall CI by hand (66), CI per city (67), interpretation (68) | Done | [06](06-inference.md) |
| **4.2 Hypothesis Formulation (H0 and H1)** | 64, 69, 72 | alpha = 0.05 (64); H0/H1 for the t-test, Furnished vs Unfurnished (69); H0/H1 for the ANOVA (72) | Done | [06](06-inference.md) |
| **4.3 Hypothesis Testing (Z-test / t-test / ANOVA)** | 70, 73 | Independent two-sample t-test (Student's, pooled variance; t and df by hand, p-value from `stats.t.sf`) Furnished vs Unfurnished (70); one-way ANOVA across six cities (73) | Done | [06](06-inference.md) |
| **4.4 Statistical Significance and Interpretation** | 71, 74 | Decision, meaning of the p-values, and the assumptions (independence, normality, confounding) for each test | Done | [06](06-inference.md) |
| **5.1 Selection of Variables** | 11, 46, 47 | Variable-type table (11); the four numerical variables chosen and why (46-47) | Done | [07](07-correlation.md) |
| **5.2 Pearson or Spearman Correlation** | 47, 49, 51 | Covariance matrix and Pearson correlation matrix (47), heatmap (49), scatter (51). Pearson only; Spearman was removed in Phase III because the requirement says "Pearson or Spearman" | Done | [07](07-correlation.md) |
| **5.3 Interpretation of Correlation Results** | 48, 50, 52 | Reading of the correlation values (48), heatmap reading (50), scatter reading (52) | Done | [07](07-correlation.md) |
| **6.1 Simple Linear Regression (mandatory)** | 75, 76, 77, 78, 79, 80, 81, 82 | Metric definitions (75), train/test split and scores function (76), method (77), fit, equation, scipy check, train/test metrics (78), fitted line (79), residual plot (80), effect of the Rs. 35 lakh listing (81), interpretation (82) | Done | [08](08-regression.md) |
| **6.2 Multiple Linear Regression (optional)** | 83, 84, 85, 86 | Design and dummy variables (83), coefficients (84), comparison of the 2 models and adjusted R-squared (85), interpretation (86) | Done | [08](08-regression.md) |
| **6.3 Regression Equation and Coefficients** | 78, 84 | Simple equation (78); coefficient table of the multiple model (84) | Done | [08](08-regression.md) |
| **6.4 Regression Plots and Interpretation** | 79, 80, 81, 82, 86 | Fitted line, residuals, interpretation | Done | [08](08-regression.md) |
| **7.1 Summary of Statistical Results** | 34, 40, 47, 56, 66, 70, 73, 78, 85, 87 | All result tables; one-page summary in doc 09 | Done | [09](09-results-summary.md) |
| **7.2 Key Findings and Observations** | 87 | Eight numbered findings | Done | [09](09-results-summary.md) |
| **7.3 Practical Applications and Implications** | none | Not code. Text for the report is in doc 12 | Text only | [12](12-limitations-and-future-scope.md) |
| **7.4 Limitations of the Study** | 88 | Limitations list | Done | [12](12-limitations-and-future-scope.md) |
| **8.1 Conclusion** | 88 | Conclusion paragraphs | Done | [09](09-results-summary.md) |
| **8.2 Future Scope** | 88 | Future scope list | Done | [12](12-limitations-and-future-scope.md) |
| **References** | none | Textbooks from the syllabus, scipy/pandas/matplotlib, Kaggle dataset | Report | [12](12-limitations-and-future-scope.md) |

## Extra content that is not in the template

| Notebook cells | Content | Why it is there |
|---|---|---|
| 53-59 | Law of Large Numbers demonstration | Group seminar topic |
| 60-63 | Central Limit Theorem demonstration | Justifies using t-tests and CIs on skewed data |

## Template-to-syllabus check

| Syllabus topic | Where it is covered |
|---|---|
| Types of data and variables | cell 11 |
| Sampling techniques, sampling bias | cell 13 (convenience sample) |
| Data cleaning and preprocessing | cells 6-7, 14-16 |
| EDA | cells 17-45 |
| Measures of central tendency and dispersion | cells 24-34 |
| Covariance and correlation (Pearson and Spearman) | cells 46-52 (Pearson only; the syllabus names Spearman too, but the project requirement says "Pearson or Spearman") |
| Sampling distribution and Central Limit Theorem | cells 60-63 |
| Estimation and confidence intervals | cells 65-68 |
| Hypothesis formulation, independent t-test, one-way ANOVA | cells 69-74 |
| Simple and multiple linear regression, regression assumptions | cells 75-86 (the constant-variance assumption is mentioned in cell 82) |
| MAE, MSE, RMSE, R-squared | cells 76-78, 85 |
| End-to-end workflow | whole notebook |
| Not covered (not applicable to this dataset) | One-sample Z-test and one-sample t-test (no benchmark value to test against), paired t-test (a paired design needs before/after data), probability distributions (Binomial/Poisson) |

## Cells that existed before Phase II (unchanged)

0-9, 14-34 (except the new box/skewness cells listed above), 39-42, 46-52, 53-59 (LLN; these were 56-62 before Phase III). The imports cell (2) and the objectives cell (1) were extended, and the key findings (87) and conclusion (88) were rewritten to include the new work.

## Report-writing gaps (not code)

Sections 1.1, 1.2, 1.4, 7.3 and the front matter have to be written as report text. The raw material is in docs 01 and 12; placeholders (names, PRNs, guide) are in `report/NOTES.md`.
