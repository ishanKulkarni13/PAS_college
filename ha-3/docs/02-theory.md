# 2. Theory and intuition (HA-3)

## 2.1 Population, sample, parameter, statistic
- **Population**: everyone we want to know about (all patients). **Sample**: the ones we actually measure.
- **Parameter** describes the population (μ, σ). **Statistic** describes the sample (x̄, s). We use statistics to estimate parameters.
- Why sample? Measuring everybody is costly or impossible. (Unit I slides 8, 31-37)

## 2.2 Sampling methods (Unit I slides 31-37)
- **Simple random sample (SRS)**: every individual has the same chance, like a raffle. Reduces bias but a small SRS can by chance miss a group.
- **Stratified sample**: split into strata (here hospitals A/B/C), take an SRS in each. Guarantees every stratum is represented. Good when strata differ from each other.
- Others in the slides: cluster and multistage sampling (slides 34-37). Systematic sampling is a standard method too (every k-th person) but is not covered in detail in the slides.
- **Bias** (e.g. non-response) can still creep in even with random selection (slide 31).

## 2.3 Sampling distribution and Central Limit Theorem (Unit III slides 14-18)
- Take many samples of size n, compute x̄ each time. The distribution of those x̄ values is the **sampling distribution of the mean**.
- Its mean = μ, its standard deviation = **standard error** SE = σ/√n.
- **CLT**: for large enough n (rule of thumb n ≥ 30) the sampling distribution of x̄ is approximately normal, **whatever the shape of the original data**.
- Intuition: averaging cancels out extremes, so averages cluster around the truth in a bell shape, and more data (bigger n) gives a narrower bell.
- In the notebook: 2000 samples of 30 gave sd of sample means 0.0948, almost equal to s/√30 = 0.0946.

## 2.4 Confidence interval (Unit III slides 21-23)
A range that likely contains the true parameter.

- Known σ: x̄ ± z × σ/√n
- Unknown σ (our case): **x̄ ± t(α/2, n−1) × s/√n**, t has n−1 degrees of freedom. t has heavier tails than z to pay for the uncertainty of estimating σ with s. For big n, t ≈ z (1.98 vs 1.96 at n = 120).
- **Correct interpretation of 95%**: if we repeated the sampling many times, about 95% of the intervals built this way would contain the true μ. It is NOT "95% probability that μ is in this particular interval" (μ is fixed, the interval is random).
- Width depends on: confidence level (higher = wider), s (more spread = wider), n (more data = narrower, shrinks like 1/√n).
- **CI for a proportion**: p̂ ± z √(p̂(1−p̂)/n). Needs np̂ and n(1−p̂) both about ≥ 10.
- Using CIs to compare: if two groups' CIs do not overlap, the means differ significantly. Overlap does NOT automatically mean no difference (B and C barely touch but the t-test is significant), so use a proper test.

## 2.5 Hypothesis testing (Unit III slides 32-53)
Steps (Unit III slide 56): state H0 and H1 → choose α → compute the test statistic → p-value (or critical value) → decide → conclude in words.

- **H0 (null)**: the "no effect / no difference" claim (e.g. μ = 4.0). **H1**: what we try to find evidence for.
- **Two-tailed**: H1 uses ≠. **One-tailed**: H1 uses < or >.
- **Test statistic**: how many standard errors the sample result is from the H0 value. z or t = (x̄ − μ0)/(s/√n).
- **p-value** (slides 37-46): probability of getting a result at least as extreme as ours *assuming H0 is true*. Small p = data are surprising under H0.
- **Decision**: p < α → reject H0; p ≥ α → fail to reject H0. We never "accept H0" or "prove" it, we just lack evidence against it.
- **α** (significance level, usually 0.05) = the Type I error rate we tolerate.
- **Type I error** (slides 47-53): rejecting H0 when it is true (false alarm), probability α. **Type II error**: failing to reject a false H0 (miss), probability β. Power = 1 − β.
- p-value is NOT the probability that H0 is true.
- **Statistically significant ≠ practically important**. With huge n tiny differences become significant; always look at the size of the difference (and effect size).

### Z-test vs t-test (Unit III slides 91-98)
| | Z-test | t-test |
|---|---|---|
| σ | known (or n large) | unknown |
| Distribution | normal | t with df |
| Use | big n, or σ given | small n or σ unknown |

- **One-sample t**: compare a sample mean with a fixed value μ0.
- **Independent two-sample t** (unpaired): two separate groups. *Pooled* version assumes equal variances; **Welch** version does not (safer). Test with Levene first.
  - t = (x̄1 − x̄2) / (sp √(1/n1 + 1/n2)), df = n1 + n2 − 2.
- **Paired t**: the same subjects measured twice; test the mean of the differences. Not applicable in this assignment.

## 2.6 Why ANOVA, and how it works
**Problem**: comparing 3 hospitals with t-tests means 3 tests; 4 age groups means 6 tests. Each test has a 5% false alarm chance, so the overall false alarm rate rises (about 14% for 3 tests, 26% for 6). This is the **multiple comparison problem**.

**One-way ANOVA** tests all means at once:
- H0: μ1 = μ2 = … = μk. H1: at least one mean differs.
- Split the total variation:
  - **SSB** (between groups) = Σ n_i (x̄_i − x̄_grand)²: how far the group means are from the overall mean.
  - **SSW** (within groups) = Σ Σ (x_ij − x̄_i)²: ordinary scatter inside the groups.
  - SST = SSB + SSW.
- **Mean squares**: MSB = SSB/(k−1), MSW = SSW/(N−k).
- **F = MSB / MSW**. If H0 is true, both estimate the same variance, so F ≈ 1. If the group means really differ, MSB is inflated and F is large.
- p-value from the F distribution with (k−1, N−k) degrees of freedom. It is always a right-tailed test.
- **Effect size η² = SSB/SST**: share of total variation explained by the grouping. About 0.01 small, 0.06 medium, 0.14 large (rule of thumb).
- Intuition: "is the difference *between* groups big compared to the normal noise *within* groups?"

**Assumptions**
1. Independent observations (different patients).
2. Each group roughly normal. Checked with **Shapiro-Wilk** (H0: data are normal; p > 0.05 is fine).
3. Equal variances (homogeneity). Checked with **Levene** (H0: equal variances; p > 0.05 is fine).
ANOVA is fairly robust to mild violations, especially with similar group sizes.

**Limitation**: a significant F only says "not all means are equal", not which ones. Hence a **post-hoc test**.

## 2.7 Tukey HSD
- Compares all pairs of means while controlling the overall (family-wise) error rate at 5%.
- Uses the studentized range distribution; reports adjusted p-values and a CI for each difference. If the CI excludes 0 (or adj p < 0.05) that pair differs.
- Why not plain t-tests per pair? They inflate Type I error. (Bonferroni is another way: α/number of comparisons.)
- Do post-hoc only if the ANOVA is significant. That is why we ran Tukey for hospitals and not for age groups.

## 2.8 Quick decision guide
| Situation | Method |
|---|---|
| One group vs a known/assumed value | one-sample t (or Z for big n) |
| Two independent groups | independent t (Levene decides pooled vs Welch) |
| Same subjects twice | paired t |
| 3+ groups, one factor | one-way ANOVA (+ Tukey if significant) |
| Data clearly non-normal, small n | Kruskal-Wallis / Mann-Whitney (non-parametric, not needed here) |
