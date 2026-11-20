# 01. Project Overview

## Topic and objective

**Topic:** Property Location and Rental Price Analysis.
**Objective:** study how monthly rent varies with the location (city) and characteristics (size, BHK, bathrooms, furnishing) of a property, and use that data to practise the main methods of the course: descriptive statistics, estimation, hypothesis testing, correlation and regression. The group seminar topic (Law of Large Numbers) is demonstrated on the same data.

**Problem statement (one paragraph, for the report):** Rent differs a lot between cities and between properties, and the averages alone do not say whether the differences are real or just chance, or how much of the variation can be explained by property characteristics. We analyse 4,746 listings from six Indian cities to describe the variation, test whether the differences are statistically significant, measure how rent relates to size, BHK and bathrooms, and build regression models that predict rent.

## Why this dataset

- It matches the syllabus project area "House Price Analysis and Prediction System" (area, bedrooms and location affect price).
- It has a numerical main variable (Rent), a categorical location variable (City) and several numerical predictors (Size, BHK, Bathroom), so every topic of the course can be applied.
- It is a real dataset (nothing was made up) with no missing values or duplicates, so little time goes into cleaning and more into analysis.

## Dataset at a glance

| Item | Value |
|---|---|
| Rows (listings) | 4,746 |
| Columns | 12 |
| Cities | Mumbai 972, Chennai 891, Bangalore 886, Hyderabad 868, Delhi 605, Kolkata 524 |
| Time window | 13 Apr 2022 to 11 Jul 2022 (about 3 months) |
| Rent range | Rs. 1,200 to Rs. 35,00,000 |
| Size range | 10 to 8,000 sq. ft. |
| Missing values / duplicates | none / none |

Columns: Posted On, BHK, Rent, Size, Floor, Area Type, Area Locality, City, Furnishing Status, Tenant Preferred, Bathroom, Point of Contact. See [03-dataset-sampling-cleaning.md](03-dataset-sampling-cleaning.md) for types and which ones are used.

## End-to-end workflow (matches the syllabus "End-to-End Data Analytics Workflow")

```
Dataset selection -> Load and check -> Clean -> EDA (stats, plots) ->
Statistical analysis (CI, tests, correlation, LLN/CLT) -> Regression modelling ->
Result interpretation -> Conclusion
```

| Step | Where in the notebook | Doc |
|---|---|---|
| Load, check, classify variables | cells 2-13 | 03 |
| Cleaning | cells 6-7, 14-16 | 03 |
| Descriptive statistics, distribution, city comparison | cells 17-45 | 04 |
| Correlation | cells 46-55 | 07 |
| LLN and CLT | cells 56-66 | 05 |
| Confidence interval and hypothesis tests | cells 67-80 | 06 |
| Regression | cells 81-93 | 08 |
| Findings and conclusion | cells 94-95 | 09, 12 |

## Key results in five sentences

1. Rent is strongly right-skewed: mean Rs. 34,993 against median Rs. 16,000, because a few very expensive listings pull the mean up.
2. Mumbai is by far the most expensive city (mean Rs. 85,321) and Kolkata the cheapest (mean Rs. 11,645); the 95% CI for the overall mean is Rs. 32,771 to Rs. 37,216.
3. All hypothesis tests are significant at 5%: Mumbai vs other cities, Furnished vs Unfurnished, and ANOVA across the six cities.
4. Rent has a moderate positive correlation with Size (Pearson 0.41), BHK (0.37) and Bathroom (0.44); Spearman values are higher (0.52, 0.57, 0.66).
5. Size alone is a weak predictor (test R-squared 0.32); a multiple regression on log(Rent) with Size, BHK, Bathroom and City is much better (test MAE about Rs. 12,230, R-squared 0.79 on the log scale).

## What we did NOT do (and why)

- **No machine-learning models.** This is a statistics course; the syllabus asks for regression and statistical tests only.
- **No removal of outliers.** Extreme rents are real listings that cannot be proven wrong. We show their effect instead (see 08).
- **No claim about all of India.** The data is a convenience sample, so "reference mean" is used instead of "population mean" (see 05).
