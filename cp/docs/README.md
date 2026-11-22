# Property Location and Rental Price Analysis: Project Docs

**Course:** Probability and Statistics (MM0805 / MM0806), VIT Pune
**Course Project Topic:** Property Location and Rental Price Analysis (syllabus project area 2: House Price Analysis and Prediction System)
**Group Seminar Topic:** Law of Large Numbers in Real-Life Decision Making
**Dataset:** [House Rent Prediction Dataset](https://www.kaggle.com/datasets/iamsouravbanerjee/house-rent-prediction-dataset) (Kaggle, iamsouravbanerjee), 4,746 listings, 6 cities, 12 columns
**Code:** [../property_location_rental_price_analysis.ipynb](../property_location_rental_price_analysis.ipynb) (one notebook, 89 cells, runs top to bottom)

## What the project does, in 6 lines

1. Cleans and describes rental listings (mean, median, mode, variance, SD, IQR, range, skewness, kurtosis).
2. Compares rent across 6 cities (table, bar chart, box plot).
3. Estimates the mean rent with a 95% confidence interval and tests whether rent differs by city and by furnishing (independent two-sample t-test, one-way ANOVA).
4. Measures relationships among Rent, Size, BHK and Bathroom (covariance and Pearson correlation).
5. Demonstrates the Law of Large Numbers (and the Central Limit Theorem) with random samples.
6. Builds simple and multiple linear regression models and evaluates them (MAE, MSE, RMSE, R-squared).

## Reading order

| # | File | Read it to know |
|---|---|---|
| 01 | [01-project-overview.md](01-project-overview.md) | The big picture: topic, objective, dataset, variables, workflow |
| 02 | [02-template-coverage.md](02-template-coverage.md) | Which notebook cells cover which report chapter/section (cell numbers) |
| 03 | [03-dataset-sampling-cleaning.md](03-dataset-sampling-cleaning.md) | Variable types, how the data was sampled, cleaning decisions |
| 04 | [04-descriptive-and-distribution.md](04-descriptive-and-distribution.md) | Descriptive statistics, skewness, kurtosis, city comparison, box plot |
| 05 | [05-lln-and-clt.md](05-lln-and-clt.md) | Law of Large Numbers and Central Limit Theorem demos |
| 06 | [06-inference.md](06-inference.md) | Confidence interval, hypotheses, independent t-test, ANOVA |
| 07 | [07-correlation.md](07-correlation.md) | Covariance and Pearson correlation |
| 08 | [08-regression.md](08-regression.md) | Simple and multiple regression, metrics, assumptions |
| 09 | [09-results-summary.md](09-results-summary.md) | Every headline number in one place |
| 10 | [10-notebook-walkthrough.md](10-notebook-walkthrough.md) | Cell-by-cell guide to the notebook |
| 11 | [11-formulas-cheatsheet.md](11-formulas-cheatsheet.md) | All formulas with a one-line meaning |
| 12 | [12-limitations-and-future-scope.md](12-limitations-and-future-scope.md) | Limitations, future scope, practical use |
| 13 | [13-phase-3-changes-from-main.md](13-phase-3-changes-from-main.md) | Only what changed between `main` and `cp-phase-3` (notebook and docs) |
| - | [VIVA.md](VIVA.md) | Likely questions from sir, with answers |
| - | [report/](report/) | Report instructions and the list of placeholders to fill |

If you only have 30 minutes before the presentation: read 09, then VIVA.md, then skim 06 and 08.

## How to run

1. Python 3 with pandas, numpy, matplotlib and scipy (no other libraries are needed).
2. The dataset is already at `cp/data/House_Rent_Dataset.csv`.
3. Open the notebook in Jupyter or VS Code, open it from the `cp` folder (it reads `data/House_Rent_Dataset.csv`), and run all cells from top to bottom.
4. Everything is reproducible: all random steps use a fixed seed (`random_state=42`, or `random_state=i` for the CLT draws).

## Statistical measures and tests used

Mean, median, mode, variance, standard deviation, IQR, range, skewness, kurtosis, standard error, 95% confidence interval (t-based), independent two-sample t-test (Student's, pooled variance), one-way ANOVA, covariance, Pearson correlation, Law of Large Numbers, Central Limit Theorem, simple linear regression, multiple linear regression, MAE, MSE, RMSE, R-squared, adjusted R-squared.

## Main variables

- **Rent**: monthly rent in rupees (main variable)
- **City**: location variable (6 cities)
- **Size**: area in sq. ft.
- **BHK**: bedrooms/hall/kitchen count
- **Bathroom**: number of bathrooms
- **Furnishing Status**: Furnished / Semi-Furnished / Unfurnished (used in one t-test)

## Status

Phase II: notebook extended to cover the whole report template, docs rewritten.

Phase III (branch `cp-phase-3`): notebook simplified (Spearman removed, defensive code removed, equal-variance caveats removed, CI wording simplified, regression caveats shortened) and these docs synced. Everything that changed, and why, is in [13-phase-3-changes-from-main.md](13-phase-3-changes-from-main.md). The report (.docx) is still to be written.
