# 09. Results Summary

> **Phase III (branch `cp-phase-3`):** cell numbers updated; the Spearman table, the equal-variance remarks and the Spearman values in the findings were removed; the conclusion paragraph said "three hypothesis tests" but there are two (t-test and ANOVA), now fixed. All other numbers are unchanged.

Every headline number, with the notebook cell it comes from. Use this page for the report (Chapter 7) and for quick answers. All values are from the executed notebook.

## Data

| Item | Value | Cell |
|---|---|---|
| Listings / columns | 4,746 / 12 | 3 |
| Missing values / duplicates | 0 / 0 | 6, 7 |
| Cities (count) | Mumbai 972, Chennai 891, Bangalore 886, Hyderabad 868, Delhi 605, Kolkata 524 | 8 |
| Date window | 13 Apr 2022 to 11 Jul 2022 | 12 |
| Rent range | Rs. 1,200 to Rs. 35,00,000 | 9 |
| Size range | 10 to 8,000 sq. ft. | 9 |

## Descriptive statistics of Rent

| Measure | Value | Cell |
|---|---|---|
| Mean | Rs. 34,993.45 | 24 |
| Median | Rs. 16,000 | 25 |
| Mode | Rs. 15,000 | 26 |
| Variance | 6,100,611,741.94 | 28 |
| Standard deviation | Rs. 78,106.41 | 29 |
| Q1, Q3, IQR | 10,000; 33,000; 23,000 | 30 |
| Min, max, range | 1,200; 35,00,000; 34,98,800 | 31 |
| IQR outliers (above 67,500) | 520 listings | 32 |
| Skewness / excess kurtosis | 21.41 / 841.11 | 36 |

## City comparison (cell 40)

| City | Mean | Median |
|---|---|---|
| Mumbai | 85,321.20 | 52,000 |
| Delhi | 29,461.98 | 17,000 |
| Bangalore | 24,966.37 | 14,000 |
| Chennai | 21,614.09 | 14,000 |
| Hyderabad | 20,555.05 | 14,000 |
| Kolkata | 11,645.17 | 8,500 |

## Estimation and tests

| Item | Result | Cell |
|---|---|---|
| 95% CI for mean rent | Rs. 32,770.75 to Rs. 37,216.16 (SE 1,133.76) | 66 |
| Furnished vs Unfurnished (independent two-sample t-test, pooled) | 56,110.31 vs 22,461.64, difference 33,648.67, pooled SD 51,678.23, df = 2,493, t = 14.482, p = 1.07e-45 | 70 |
| One-way ANOVA, 6 cities | F = 117.64 (df 5 and 4,740), p = 1.27e-117 | 73 |

Both reject H0 at alpha = 0.05.

## Correlation (cell 47)

| Pair | Pearson |
|---|---|
| Rent and Size | 0.4136 |
| Rent and BHK | 0.3697 |
| Rent and Bathroom | 0.4412 |
| Size and BHK | 0.7161 |
| Size and Bathroom | 0.7407 |
| BHK and Bathroom | 0.7949 |

Rent-Size covariance: 20,485,348.00 (cell 47). Spearman values were removed from the notebook; see [07](07-correlation.md).

## LLN and CLT

| Item | Result | Cell |
|---|---|---|
| Reference mean | Rs. 34,993.45 | 54 |
| Sample mean at n = 10 / 100 / 1000 / 4000 | 60,850.00 / 41,134.99 / 34,495.06 / 35,394.75 | 56 |
| CLT, n = 30: spread of means (formula) | 12,748.17 (14,260.21) | 61 |
| CLT, n = 100: spread of means (formula) | 7,274.49 (7,810.64) | 61 |

## Regression (cells 78, 84, 85)

| Model | Equation / key terms | Train R-sq | Test R-sq | Test MAE | Test RMSE |
|---|---|---|---|---|---|
| Simple | Rent = -12,678.18 + 49.46 x Size | 0.1488 | 0.3172 | 27,807 | 52,189 |
| Multiple, Rent | Size 38.28, BHK 3,450, Bathroom 12,035, Mumbai +58,371 vs Kolkata | 0.2806 | 0.5204 | 22,345 | 43,739 |

Multiple model adjusted R-sq (train): 0.2791.
Effect of the Rs. 35 lakh Bangalore listing on the simple model: train R-sq 0.1488 with it, 0.2466 without; slope 49.46 with, 46.06 without (cell 81).

## Key findings (report 7.2)

1. 4,746 listings, 6 cities, 12 columns, no missing values or duplicates.
2. Rent is strongly right-skewed (skewness about 21): mean Rs. 34,993 is much higher than median Rs. 16,000.
3. Mumbai has the highest average rent (Rs. 85,321) and Kolkata the lowest (Rs. 11,645): location is associated with large rent differences.
4. 95% CI for the mean rent: Rs. 32,771 to Rs. 37,216.
5. Both tests reject H0 at 5%: the independent two-sample t-test shows furnished properties cost more than unfurnished ones (gap about Rs. 33,649, t = 14.48, p about 1e-45), and the one-way ANOVA shows the mean rent is not the same across the six cities (F = 117.64).
6. Rent correlates moderately with Size, BHK and Bathroom (Pearson 0.37 to 0.44). Size, BHK and Bathroom are strongly correlated with each other (0.72 to 0.79).
7. LLN: sample means stabilise around the reference mean as n grows; CLT: the spread of sample means shrinks as n grows.
8. Size alone is a weak predictor; the multiple model on Rent predicts better (test MAE about Rs. 22,345, test R-squared 0.52).

## Conclusion paragraph (report 8.1, short form)

Rental prices in the dataset are strongly spread and right-skewed, and location is associated with large differences: Mumbai is the most expensive city and Kolkata the cheapest. Confidence intervals and two hypothesis tests confirm that rent differs significantly by city and by furnishing status. Rent has a moderate positive association with size, BHK and bathrooms, and a multiple regression on Rent with those variables and city predicts better than size alone on unseen listings (test R-squared 0.52 against 0.32), although much of the variation in rent remains unexplained. The Law of Large Numbers and the Central Limit Theorem were illustrated with random samples. All relationships are descriptive, not causal, and the data is a non-random collection of listings from six cities in 2022.
