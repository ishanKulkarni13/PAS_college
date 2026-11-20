# 07. Covariance and Correlation

Notebook cells 46-55. Report sections 5.1 to 5.3.

## Selection of variables (report 5.1)

Covariance and correlation need **numerical** variables, so we use the four numerical columns: **Rent, Size, BHK, Bathroom**. City and Furnishing Status are categorical and cannot go into these formulas directly (they are handled by t-tests, ANOVA and dummy variables instead). All six unique pairs are computed on the same 4,746 complete rows.

## Covariance (cell 47)

cov(X, Y) = sum (x - x-bar)(y - y-bar) / (n - 1)

Positive: the variables tend to rise together. Negative: one rises as the other falls. The size of the number depends on the units, so it cannot be compared between pairs.

| Pair | Covariance |
|---|---|
| Rent and Size | 20,485,348.00 |
| Rent and BHK | 24,033.31 |
| Rent and Bathroom | 30,482.50 |
| Size and BHK | 377.99 |
| Size and Bathroom | 415.51 |
| BHK and Bathroom | 0.59 |

All covariances are positive. (Rent-Size looks huge only because Rent is in rupees and Size in square feet.)

## Pearson correlation (cell 47)

r = cov(X, Y) / (s_X x s_Y), between -1 and +1. Measures the strength of the **straight-line** relationship.

Strength bands used in the notebook (rough guide, not a universal rule): |r| below 0.10 negligible, 0.10 to 0.30 weak, 0.30 to 0.50 moderate, 0.50 or more strong.

## Spearman correlation (cells 53-55)

Spearman correlation is Pearson's correlation applied to the **ranks** of the values. It measures whether one variable consistently rises when the other does (a *monotonic* relationship), not necessarily a straight line, and it is not distorted by extreme values.

## Pearson vs Spearman (cell 54)

| Pair | Pearson | Spearman | Pearson strength |
|---|---|---|---|
| Rent and Size | 0.4136 | 0.5214 | moderate |
| Rent and BHK | 0.3697 | 0.5684 | moderate |
| Rent and Bathroom | 0.4412 | 0.6630 | moderate |
| Size and BHK | 0.7161 | 0.7551 | strong |
| Size and Bathroom | 0.7407 | 0.7221 | strong |
| BHK and Bathroom | 0.7949 | 0.7906 | strong |

## Interpretation (report 5.3)

1. **Rent is moderately related to Size, BHK and Bathroom** (Pearson 0.37 to 0.44). Bigger properties with more rooms tend to cost more, but the relationship is far from perfect: city, furnishing and location quality also matter.
2. **Spearman is higher than Pearson for every pair that includes Rent** (e.g. Rent-Bathroom 0.66 vs 0.44). Rent rises consistently with these attributes, but extreme rents drag the Pearson value down. When data has outliers, Spearman describes the pattern better; this is why we report both.
3. **Size, BHK and Bathroom are strongly correlated with each other** (0.72 to 0.79): bigger flats have more bedrooms and bathrooms. This is **multicollinearity**, which matters for regression (see 08): the individual coefficients of these three variables are less stable.
4. **Correlation is not causation.** A moderate Rent-Size correlation does not mean adding area *causes* rent to rise; other factors (city, building quality) move with both.
5. **Correlation measures linear/monotonic association only.** A value near zero would not rule out a curved relationship.

## The plots

- **Heatmap (cell 49):** colour-coded correlation matrix; red = positive, blue = negative, numbers printed in each cell.
- **Scatter plot Size vs Rent (cell 51):** each dot is a listing; the upward drift matches r = 0.41, and the wide scatter shows size alone does not fix the rent. The y-axis is cut at Rs. 5,00,000 for readability.

## Covariance vs correlation (very likely viva question)

| | Covariance | Correlation |
|---|---|---|
| Range | any number | -1 to +1 |
| Depends on units? | Yes | No (unit-free) |
| Comparable across pairs? | No | Yes |
| Tells | direction | direction and strength |
| Relation | | r = cov / (s_X x s_Y) |
