# 11. Formulas Cheat Sheet

> **Phase III (branch `cp-phase-3`):** the notebook no longer uses Spearman (the row is kept here because the syllabus lists it), and the equal-variance remark was shortened.

n = number of observations, x-bar = sample mean, s = sample standard deviation.

## Descriptive statistics

| Measure | Formula | Meaning |
|---|---|---|
| Mean | x-bar = (sum x) / n | Balance point; pulled by extremes |
| Median | middle value of the sorted data (average of the two middle values if n is even) | Typical value; robust to extremes |
| Mode | most frequent value | Most common value |
| Sample variance | s^2 = sum (x - x-bar)^2 / (n - 1) | Spread, in squared units |
| Standard deviation | s = sqrt(s^2) | Spread, in original units |
| Range | max - min | Total spread |
| IQR | Q3 - Q1 | Spread of the middle 50% |
| Outlier fences | Q1 - 1.5 IQR and Q3 + 1.5 IQR | Points outside are flagged |
| Skewness | average of ((x - x-bar)/s)^3 (pandas uses a small-sample correction) | > 0 long right tail |
| Excess kurtosis | average of ((x - x-bar)/s)^4 minus 3 | > 0 heavier tails than normal |

Why n - 1 (Bessel's correction): x-bar is estimated from the same data, which makes deviations slightly too small; dividing by n - 1 corrects that and makes s^2 an unbiased estimate of the population variance.

## Sampling, LLN and CLT

| Item | Formula / statement |
|---|---|
| Standard error of the mean | SE = s / sqrt(n) |
| LLN | x-bar converges to the true mean mu as n grows |
| CLT | x-bar is approximately Normal(mu, sigma^2 / n) for large n, whatever the data's shape |
| Z score | z = (x - mu) / sigma |

## Confidence interval for a mean

x-bar +/- t(alpha/2, n-1) x s / sqrt(n). For 95%: alpha = 0.05; for large n, t* is about 1.96. Interval width shrinks as n grows or s shrinks.

## Hypothesis tests

| Test | Statistic | Degrees of freedom |
|---|---|---|
| One-sample Z | z = (x-bar - mu0) / (sigma / sqrt(n)) | none (needs known sigma) |
| One-sample t | t = (x-bar - mu0) / (s / sqrt(n)) | n - 1 |
| **Independent t, pooled (ours)** | t = (x1-bar - x2-bar) / (s_p sqrt(1/n1 + 1/n2)), s_p^2 = [(n1-1)s1^2 + (n2-1)s2^2]/(n1+n2-2) | n1 + n2 - 2 |
| Paired t | t = d-bar / (s_d / sqrt(n)) on the differences | n - 1 |
| **One-way ANOVA (ours)** | F = MSB / MSW; MSB = SSB/(k-1); MSW = SSW/(N-k) | k - 1 and N - k |

- Decision: reject H0 if p-value < alpha (0.05).
- Both the pooled t-test and one-way ANOVA assume independent observations and roughly normal data (the textbook versions also assume equal variances).

## Covariance and correlation

| Item | Formula |
|---|---|
| Covariance | cov(X,Y) = sum (x - x-bar)(y - y-bar) / (n - 1) |
| Pearson r | r = cov(X,Y) / (s_X s_Y), -1 <= r <= +1 |
| Spearman (syllabus topic, not used in the notebook) | Pearson r computed on the ranks of X and Y |
| R-squared in simple regression | R-squared = r^2 |

## Regression

| Item | Formula |
|---|---|
| Simple model | y = b0 + b1 x |
| Slope | b1 = sum (x - x-bar)(y - y-bar) / sum (x - x-bar)^2 = r s_y / s_x |
| Intercept | b0 = y-bar - b1 x-bar |
| Multiple model | y = b0 + b1 x1 + ... + bk xk; coefficients solve beta = (X'X)^-1 X'y (least squares) |
| Residual | e = y - y-hat |
| SSE, SST | SSE = sum e^2; SST = sum (y - y-bar)^2 |
| R-squared | 1 - SSE / SST |
| Adjusted R-squared | 1 - (1 - R-squared)(n - 1) / (n - k - 1) |
| MAE | mean of abs(e) |
| MSE | mean of e^2 |
| RMSE | sqrt(MSE) |

## Probability distributions (syllabus; not used in this project's analysis)

| Distribution | Use | Key formula |
|---|---|---|
| Binomial | n trials, success prob p | P(X = k) = C(n,k) p^k (1-p)^(n-k) |
| Poisson | counts in an interval, rate lambda | P(X = k) = e^-lambda lambda^k / k! |
| Normal | bell curve, mean mu, SD sigma | z = (x - mu)/sigma |

These are not applied to the rent data (rent is continuous and skewed, not a count), but may be asked about in the viva as course topics.
