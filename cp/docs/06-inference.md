# 06. Statistical Inference: Confidence Interval and Hypothesis Tests

> **Phase III (branch `cp-phase-3`):** the equal-variance caveats were removed from the notebook and from this doc (they used to be in the t-test text, the ANOVA text, the key findings and the limitations), and the CI interpretation was simplified. Be ready for the question "why not Welch's t-test?" (answer in VIVA Q51). Details: [13](13-phase-3-changes-from-main.md).

Notebook cells 64-74. Report sections 4.1 to 4.4.

**Inference = using a sample to say something about a bigger group.** Here the "bigger group" is the rental market, and our sample is the 4,746 listings. Since it is a convenience sample (see 03), conclusions describe these listings and are a guide to the wider market.

## Estimation and the 95% confidence interval (cells 65-68)

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

The notebook computes this by hand from the formula above (cell 66).

**Why t and not Z?** The population standard deviation is unknown, so we use s and the t distribution. With 4,745 degrees of freedom t* is practically 1.96, so the result is almost the same as a Z interval.

**Interpretation (as written in the notebook):** we are 95% confident that the mean rent lies in this range.

**If sir pushes for the formal meaning:** if we repeated the sampling many times and built an interval each time, about 95% of those intervals would contain the true mean. The notebook no longer spells this out; it was toned down on purpose (see the Phase III note above).

### CI by city (cell 67)

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

## Test 1: Independent two-sample t-test (Student's), Furnished vs Unfurnished (cells 69-71)

**Intuition.** We have two *independent* groups of properties (furnished and unfurnished). We want to know whether their population mean rents are different, or whether the gap in our sample could just be sampling chance.

- H0: mu_furnished = mu_unfurnished
- H1: mu_furnished is not equal to mu_unfurnished (two-sided)

**This is the traditional (Student's) pooled method.** The population standard deviation is unknown, so we pool the two sample variances into one common estimate:

s_p^2 = [(n1 - 1) s1^2 + (n2 - 1) s2^2] / (n1 + n2 - 2)

t = (x1-bar - x2-bar) / ( s_p x sqrt(1/n1 + 1/n2) ), with n1 + n2 - 2 degrees of freedom, from the Student t distribution.

In the notebook the pooled variance, pooled SD, standard error, t statistic and degrees of freedom are all calculated by hand. SciPy's Student's t distribution is used only for the two-tailed p-value, `p = 2 * stats.t.sf(abs(t), df)`, using our own t and df, and p is compared with alpha = 0.05. We use the traditional pooled test taught in the course; Welch's version is not used.

| | Furnished | Unfurnished |
|---|---|---|
| n | 680 | 1,815 |
| Mean | 56,110.31 | 22,461.64 |
| Median | 27,000 | 12,000 |
| Std dev | 80,630.84 | 35,167.73 |

| Quantity | Value |
|---|---|
| Difference of means | Rs. 33,648.67 |
| Pooled standard deviation | Rs. 51,678.23 |
| Degrees of freedom (680 + 1,815 - 2) | 2,493 |
| t statistic (by hand) | 14.482 |
| p-value (two-tailed, from the t distribution with 2,493 df) | 1.07e-45 |

**Decision:** p << 0.05, reject H0. Furnished properties have a clearly higher mean rent (about Rs. 33,649 more). The medians (27,000 vs 12,000) show the same direction, so it is not only caused by extreme rents.

**Assumptions:** independent observations (each listing is a different property, reasonable) and roughly normal data. Rent is strongly skewed, so we rely on the large group sizes and the CLT instead of normal data.

**Caution on meaning:** this is a raw two-group comparison. Furnished flats may also be larger or more often in Mumbai (confounding), so the effect of furnishing alone is probably smaller than the raw gap. It shows association, not cause.

### Which t-test is it? (viva)

| Test | Use | Ours? |
|---|---|---|
| One-sample t / Z | One group against a stated value | No |
| **Independent two-sample t** | Two *different* groups | **Yes** |
| Paired t | The *same* units measured twice (before/after) | No (no paired data) |
| Z-test | Needs a known population SD | No (SD unknown; with this n it would give almost the same answer as t) |

## Test 2: One-way ANOVA across six cities (cells 72-74)

- H0: mean rent is equal across all six cities (mu1 = mu2 = ... = mu6)
- H1: at least one city's mean rent is different

| Quantity | Value |
|---|---|
| Groups (k) / observations (N) | 6 / 4,746 |
| Degrees of freedom | 5 (between) and 4,740 (within) |
| F statistic | 117.64 |
| p-value | 1.27e-117 |

**Idea of ANOVA:** compare the variation *between* the city means (MSB) with the variation *within* the cities (MSW): F = MSB / MSW, MSB = SSB/(k - 1), MSW = SSW/(N - k). A large F means the city means differ more than chance within the cities would explain.

**Decision:** reject H0: the mean rent is not the same in all six cities.

**What a significant ANOVA does and does not say:** it says that *at least one* city's mean differs. It does **not** say that every city differs from every other city, and it does not say which cities differ. The city means, CIs and box plot suggest Mumbai is far above the rest and Kolkata is at the other end, but we did not run a separate test for each pair (no post-hoc test).

**Why ANOVA and not many t-tests?** With 6 groups there are 15 pairs; testing each at 5% raises the chance of at least one false positive. ANOVA tests all groups in one go.

**Assumptions:**

| Assumption | Status |
|---|---|
| Independent observations | Plausible |
| Roughly normal data in each city | Not exactly true; rent is strongly skewed, so we rely on the large groups (524 to 972 listings) and the CLT |

The size of F (117.64) and the clear gaps between the city means make the conclusion "rent differs by city" very plausible.

## Summary of the tests

| Test | H0 | Statistic | p-value | Decision |
|---|---|---|---|---|
| Independent two-sample t (Student's, pooled), Furnished vs Unfurnished | equal means | t = 14.48, df = 2,493 | 1.07e-45 | Reject |
| One-way ANOVA, 6 cities | all means equal | F = 117.64, df = 5 and 4,740 | 1.27e-117 | Reject |
