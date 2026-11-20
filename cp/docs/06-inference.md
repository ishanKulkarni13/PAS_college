# 06. Statistical Inference: Confidence Interval and Hypothesis Tests

Notebook cells 67-80. Report sections 4.1 to 4.4.

**Inference = using a sample to say something about a bigger group.** Here the "bigger group" is the rental market, and our sample is the 4,746 listings. Since it is a convenience sample (see 03), conclusions describe these listings and are a guide to the wider market.

## Estimation and the 95% confidence interval (cells 68-71)

**Point estimate:** the sample mean, Rs. 34,993.45.
**Interval estimate (95% CI):**

CI = x-bar +/- t* x s / sqrt(n)

| Piece | Value |
|---|---|
| x-bar | 34,993.45 |
| s | 78,106.41 |
| n | 4,746 |
| Standard error s/sqrt(n) | 1,133.76 |
| t* = t(0.975, 4745 df) | 1.9605 (about 1.96, like Z, because n is large) |
| Margin of error | about 2,222.7 |
| **95% CI** | **Rs. 32,770.75 to Rs. 37,216.16** |

The notebook computes this by hand and checks it with `scipy.stats.t.interval`; both agree exactly (cell 69).

**Why t and not Z?** The population standard deviation is unknown, so we use s and the t distribution. With 4,745 degrees of freedom t* is practically 1.96, so the result is almost the same as a Z interval.

**Correct interpretation:** if we repeated the sampling many times and built an interval each time, about 95% of those intervals would contain the true mean. It is *wrong* to say "there is a 95% probability that the true mean is in this interval": the true mean is fixed, the interval is what varies.

### CI by city (cell 70)

| City | Count | Mean | CI lower | CI upper |
|---|---|---|---|---|
| Mumbai | 972 | 85,321.20 | 78,867.83 | 91,774.58 |
| Delhi | 605 | 29,461.98 | 25,985.42 | 32,938.55 |
| Bangalore | 886 | 24,966.37 | 17,050.29 | 32,882.44 |
| Chennai | 891 | 21,614.09 | 19,439.72 | 23,788.46 |
| Hyderabad | 868 | 20,555.05 | 18,793.91 | 22,316.19 |
| Kolkata | 524 | 11,645.17 | 10,689.35 | 12,600.99 |

- Mumbai's interval does not overlap any other city's: it is clearly the most expensive.
- Kolkata's interval is the lowest.
- Bangalore's interval is very wide because of the single Rs. 35,00,000 listing (large SD).
- Narrower interval = larger n and smaller SD.

## Hypothesis testing: the general recipe

1. State H0 (no difference / no effect) and H1 (there is a difference).
2. Choose the significance level alpha = 0.05.
3. Compute the test statistic and the p-value.
4. Decide: if p < alpha, reject H0; otherwise fail to reject H0.
5. Interpret in plain words.

**p-value:** the probability of getting a result at least this extreme *if H0 were true*. Small p means the data are hard to explain by chance alone. It is **not** the probability that H0 is true, and it does not measure the size of the effect.

**Type I error:** rejecting a true H0 (probability = alpha = 5%). **Type II error:** failing to reject a false H0. Statistical significance is not the same as practical importance: with n = 4,746 even small differences become significant, so we also report the size of the difference in rupees.

## Test 1: Welch t-test, Mumbai vs other cities (cells 72-74)

- H0: mean rent in Mumbai = mean rent in the other five cities
- H1: the means are different (two-sided)

| | Mumbai | Other cities |
|---|---|---|
| n | 972 | 3,774 |
| Mean | 85,321.20 | 22,031.45 |
| Difference | **63,289.75** | |
| t statistic | 18.337 | |
| p-value | 3.02e-66 | |

**Decision:** p << 0.05, reject H0. Mumbai's mean rent is about Rs. 63,000 higher. (The test says the gap is real, not why it exists.)

## Test 2: Welch t-test, Furnished vs Unfurnished (cells 75-77)

- H0: mean rent of furnished = mean rent of unfurnished
- H1: different

| | Furnished | Unfurnished |
|---|---|---|
| n | 680 | 1,815 |
| Mean | 56,110.31 | 22,461.64 |
| Median | 27,000 | 12,000 |
| Difference of means | **33,648.67** | |
| t statistic | 10.514 | |
| p-value | 2.88e-24 | |

**Decision:** reject H0. The medians show the same gap, so it is not only caused by extreme values. **Caution:** furnished flats may also be larger or more often in Mumbai (confounding), so the pure effect of furnishing is probably smaller than the raw gap. Semi-Furnished (2,251 listings) is not part of this test.

## Why Welch's t-test and not the ordinary (pooled) t-test?

The ordinary independent t-test assumes both groups have the same variance. Rent in Mumbai has a much larger spread than in other cities (and furnished vs unfurnished also differ), so equal variances are not believable. Welch's version does not assume equal variances and adjusts the degrees of freedom (Welch-Satterthwaite; here 1,175.2 and 777.8 instead of n1 + n2 - 2). When variances are equal it gives almost the same answer, so it is the safer default.

**Independent vs paired t-test (syllabus):** independent = two *different* groups (our case). Paired = the *same* units measured twice (before/after). We have no paired data, so no paired test.

**Z-test (syllabus):** needs a known population SD. We do not have one, so t-tests are the correct choice; with n this large they coincide with Z tests anyway.

## Test 3: One-way ANOVA across six cities (cells 78-80)

- H0: mean rent is equal in all six cities
- H1: at least one city's mean rent differs

| Test | Statistic | p-value |
|---|---|---|
| One-way ANOVA | F = 117.64 | 1.27e-117 |
| Kruskal-Wallis (robustness check) | H = 1,613.67 | 0 (below what the computer can store) |
| Levene (equal variances?) | 55.38 | 4.17e-56 |

**Idea of ANOVA:** compare the variation *between* city means with the variation *within* cities: F = MSB / MSW. A large F means the city means differ more than chance would allow.

**Decision:** reject H0: not all cities have the same mean rent. ANOVA only says "at least one differs", not which; the box plot and CIs show Mumbai is the main reason and Kolkata the other extreme. (A post-hoc test such as Tukey would name every pair; not done here.)

**Assumptions and what we did:**
| Assumption | Status | What we did |
|---|---|---|
| Independent observations | Plausible | none needed |
| Normal data in each group | Not true (rent is very skewed) | Large n and CLT; Kruskal-Wallis cross-check |
| Equal variances | **Violated** (Levene p ~ 0) | Kruskal-Wallis cross-check, and Welch t-tests for the two-group comparisons |

Both ANOVA and Kruskal-Wallis reject H0, so the conclusion does not depend on the assumption.

**Why ANOVA and not many t-tests?** With 6 groups there are 15 pairs; testing each at 5% inflates the chance of at least one false positive. ANOVA tests all groups in one go.

## Summary of the tests

| Test | H0 | Statistic | p-value | Decision |
|---|---|---|---|---|
| Welch t: Mumbai vs others | equal means | t = 18.34 | 3.0e-66 | Reject |
| Welch t: Furnished vs Unfurnished | equal means | t = 10.51 | 2.9e-24 | Reject |
| One-way ANOVA, 6 cities | all means equal | F = 117.64 | 1.3e-117 | Reject |
| Kruskal-Wallis, 6 cities | same distribution | H = 1613.67 | about 0 | Reject |
