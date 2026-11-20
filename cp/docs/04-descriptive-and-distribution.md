# 04. Descriptive Statistics, Distribution and City Comparison

Notebook cells 17-45. Report sections 3.1 to 3.4.

## Descriptive statistics of Rent (report 3.1)

| Measure | Value | Cell | Meaning |
|---|---|---|---|
| Count | 4,746 | 18 | Number of listings |
| Mean | Rs. 34,993.45 | 24 | Sum of rents / 4,746 |
| Median | Rs. 16,000 | 25 | Middle value when sorted |
| Mode | Rs. 15,000 | 26 | Most frequent rent |
| Variance | 6,100,611,741.94 (Rs. squared) | 28 | Average squared distance from the mean (divide by n-1) |
| Standard deviation | Rs. 78,106.41 | 29 | Square root of variance, same unit as Rent |
| Q1 / Q3 | Rs. 10,000 / Rs. 33,000 | 30 | 25th and 75th percentiles |
| IQR | Rs. 23,000 | 30 | Q3 - Q1, spread of the middle 50% |
| Minimum / Maximum | Rs. 1,200 / Rs. 35,00,000 | 31 | Extremes |
| Range | Rs. 34,98,800 | 31 | Max - Min |

**Other variables (cell 34):**

| Variable | Mean | Median | Mode | Std Dev | Min | Max |
|---|---|---|---|---|---|---|
| Rent (Rs.) | 34,993.45 | 16,000 | 15,000 | 78,106.41 | 1,200 | 35,00,000 |
| Size (sq. ft.) | 967.49 | 850 | 1,000 | 634.20 | 10 | 8,000 |
| BHK | 2.08 | 2 | 2 | 0.83 | 1 | 6 |
| Bathroom | 1.97 | 2 | 2 | 0.88 | 1 | 10 |

**Sample vs population formula:** pandas `.var()` and `.std()` divide by n-1 (sample variance). We use the sample version because the data is a sample. The population version divides by n; with n = 4,746 the difference is tiny.

**Why mean (34,993) is more than twice the median (16,000):** a few very expensive listings pull the mean upward but barely move the median. So for a "typical" rent the median is the better number; the mean is still needed for the confidence interval, t-tests and LLN.

## Distribution analysis (report 3.2)

| Measure | Rent | log(Rent) | Normal distribution |
|---|---|---|---|
| Skewness | 21.41 | 0.91 | 0 |
| Excess kurtosis | 841.11 | 0.88 | 0 |

- **Skewness** measures asymmetry. Positive means a long right tail. 21 is extreme.
- **Excess kurtosis** measures how heavy the tails are (pandas `.kurt()` is "excess": normal = 0). 841 means a very heavy tail caused by a few giant values.
- **Log transform:** taking natural log compresses large values. log(Rent) has skewness 0.91, so it is much closer to a bell shape. This is why the regression also uses log(Rent) (see 08).
- **Histogram (cell 21):** heavily right-skewed; the x-axis is cut at Rs. 3,00,000 for readability (it is stated in the chart title on the side-by-side version, cell 37).

**Is Rent normal?** No. We did not need to run a normality test: skewness 21 and kurtosis 841 make it obvious. For the CI and tests we rely on the large sample (CLT, see 05).

## Outliers (cell 32)

IQR rule: outliers are below Q1 - 1.5 x IQR = -24,500 or above Q3 + 1.5 x IQR = 67,500. 520 listings (about 11%) are above the upper bound. They are kept (see 03).

## Location-wise rent: city comparison (cells 40-45)

| City | Count | Mean | Median | Std |
|---|---|---|---|---|
| Mumbai | 972 | 85,321.20 | 52,000 | 102,525.12 |
| Delhi | 605 | 29,461.98 | 17,000 | 43,542.05 |
| Bangalore | 886 | 24,966.37 | 14,000 | 120,056.17 |
| Chennai | 891 | 21,614.09 | 14,000 | 33,069.91 |
| Hyderabad | 868 | 20,555.05 | 14,000 | 26,436.20 |
| Kolkata | 524 | 11,645.17 | 8,500 | 11,137.49 |

- Mumbai's average is about 7 times Kolkata's.
- **Bangalore's SD (120,056) is the largest even though its mean is only 24,966.** The reason is one listing of Rs. 35,00,000. It inflates Bangalore's SD, its CI width and the raw regression (see 06 and 08).
- Chennai, Hyderabad and Bangalore have the same median (14,000).
- **Bar chart (cell 41)** shows mean rent by city. **Box plot (cell 44)** shows the whole distribution per city on a log y-axis; Mumbai's box sits highest, Kolkata's lowest, and every city has many points above the upper whisker.

**Why the box plot uses a log axis:** with Rs. 35,00,000 on the axis the boxes would be squashed flat. A log axis keeps equal ratios equal, so the boxes are readable. It is labelled on the chart.

### How to read a box plot (viva)
Box = Q1 to Q3 (the middle 50%), line inside = median, whiskers reach up to 1.5 x IQR beyond the box, dots beyond the whiskers = outliers.

## Observations to say out loud

1. Rent is right-skewed, so mean > median.
2. Location matters a lot: Mumbai is far above everyone else, Kolkata far below.
3. Within-city variation is also huge (large SD), so city explains part of rent but not all.
4. Skew and outliers are the reason we also use median, Spearman, log(Rent), Welch and Kruskal-Wallis.
