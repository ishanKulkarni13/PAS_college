# 4. Results and how to say them in words

All values are from the executed `ha-3.ipynb`.

## Data
120 patients; hospitals A = 34, B = 42, C = 44; age groups 18-30 = 30, 31-45 = 26, 46-60 = 27, 60+ = 37. No missing values or duplicates.

| Group | n | Mean | Std |
|---|---|---|---|
| All | 120 | 3.976 | 0.518 |
| Hospital A | 34 | 4.015 | 0.453 |
| Hospital B | 42 | 3.771 | 0.547 |
| Hospital C | 44 | 4.141 | 0.480 |
| Age 18-30 | 30 | 3.913 | 0.529 |
| Age 31-45 | 26 | 4.077 | 0.445 |
| Age 46-60 | 27 | 3.948 | 0.627 |
| Age 60+ | 37 | 3.976 | 0.479 |

## Samples
- SRS of 40: mean 3.950. Stratified of 40: mean 3.9575. Full data: 3.9758. Close, so both are fair samples.
- CLT simulation: mean of 2000 sample means 3.9778, sd 0.0948, theoretical SE 0.0946.

## Confidence intervals (95%)
| Group | CI |
|---|---|
| Overall | (3.882, 4.069) |
| A | (3.857, 4.173) |
| B | (3.601, 3.942) |
| C | (3.995, 4.287) |
| 18-30 | (3.716, 4.111) |
| 31-45 | (3.897, 4.257) |
| 46-60 | (3.700, 4.196) |
| 60+ | (3.816, 4.135) |

**Say it like this:** "We are 95% confident that the true mean satisfaction of all patients is between 3.88 and 4.07." The age group intervals overlap almost entirely; B and C only just touch.

Other levels (overall mean): 90% (3.897, 4.054), 99% (3.852, 4.100). Wider for higher confidence.
Proportion satisfied (score ≥ 4): 61/120 = 0.508, 95% CI (0.419, 0.598).

## Hypothesis tests (α = 0.05)
| Test | Statistic | p | Decision |
|---|---|---|---|
| One-sample Z (μ0 = 4.0) | z = −0.511 | 0.609 | fail to reject H0 |
| One-sample t (df = 119) | t = −0.511 | 0.610 | fail to reject H0 |
| B vs C, pooled t | t = −3.334 | 0.0013 | reject H0 |
| B vs C, Welch | t = −3.324 | 0.0013 | reject H0 |
| 18-30 vs 60+, pooled t | t = −0.506 | 0.615 | fail to reject H0 |

Critical values at 5%: z = ±1.96, t(119) = ±1.98. |−0.511| is smaller, consistent with the large p.

**Say it like this (p-value):** "p = 0.0013 means that if Hospitals B and C really had the same mean satisfaction, we would see a gap this big only about 0.13% of the time. That is below 5%, so we reject H0 and conclude the difference is statistically significant."
**And for the non-significant one:** "p = 0.61 is far above 0.05, so there is no evidence the average differs from 4.0. This does not prove it equals 4.0."

## One-way ANOVA
| Factor | SSB | SSW | df | MSB | MSW | F | p | η² | Decision |
|---|---|---|---|---|---|---|---|---|---|
| Hospital | 3.005 | 28.935 | 2, 117 | 1.503 | 0.247 | 6.076 | 0.0031 | 0.094 | reject H0 |
| Age group | 0.404 | 31.536 | 3, 116 | 0.135 | 0.272 | 0.495 | 0.687 | 0.013 | fail to reject H0 |

- Hospital: F = 6.08 means the variation between hospital means is about 6 times the within-hospital noise. p = 0.0031 < 0.05. At least one hospital mean differs.
- Age: F = 0.49 < 1 means between-group variation is even smaller than the noise. p = 0.687, no evidence of a difference.
- η² = 0.094 means hospital explains about 9.4% of the variation in scores; the other 90% is individual differences. A small to medium effect.

## Assumption checks
- Levene (equal variance): hospital p = 0.760, age p = 0.617. Both > 0.05, so equal variance is fine.
- Shapiro-Wilk (normality) per group: hospitals A 0.797, B 0.647, C 0.171; ages 0.584, 0.765, 0.418, 0.251. All > 0.05, so normality is fine.

## Tukey HSD (hospitals)
| Pair | Mean diff | p (adjusted) | 95% CI | Significant |
|---|---|---|---|---|
| A - B | +0.243 | 0.090 | (−0.029, 0.516) | no |
| A - C | −0.126 | 0.509 | (−0.396, 0.143) | no |
| B - C | −0.369 | 0.0023 | (−0.624, −0.115) | **yes** |

Only the B-C interval excludes 0. A is in the middle: not distinguishable from B or from C.

## Final conclusion in one paragraph (memorise)
"Mean patient satisfaction is about 3.98 (95% CI 3.88 to 4.07). Satisfaction differs significantly between hospitals (one-way ANOVA F = 6.08, p = 0.0031), and Tukey's test shows the difference is between Hospital B (lowest, 3.77) and Hospital C (highest, 4.14). It does not differ significantly between age groups (F = 0.49, p = 0.687). ANOVA assumptions were satisfied. The data are observational, so we know B is lower but not why."

## Small things worth knowing
- A vs B had mean difference 0.243 with Tukey p = 0.090, but a plain two-sample t-test gives about 0.041. This is exactly why we use Tukey: multiple plain t-tests inflate false positives.
- p-values in the notebook for B vs C differ slightly between the t-test (0.0013) and Tukey (0.0023) because Tukey adjusts for the 3 comparisons.
- Results depend on the random seeds only for the sampling part (SRS, stratified, CLT); ANOVA / tests use all 120 rows so they never change.
