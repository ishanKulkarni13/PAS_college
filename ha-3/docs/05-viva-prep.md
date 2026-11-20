# 5. Viva preparation (HA-3)

Answers are short on purpose: say the one-line answer first, then add detail if Sir wants more.

## A. About the assignment

**Q. What was the aim?**
To check whether patient satisfaction differs significantly across hospitals and age groups, using samples, confidence intervals, hypothesis tests and one-way ANOVA.

**Q. Which dataset did you use?**
A survey of 120 patients: Hospital (A/B/C), Age_Group (4 groups), Satisfaction_Score (1 to 5 scale, observed 2.4 to 5.0).

**Q. What is your final conclusion?**
Satisfaction differs significantly between hospitals (F = 6.08, p = 0.0031), specifically B is lower than C (Tukey p = 0.0023). It does not differ across age groups (F = 0.49, p = 0.687).

## B. Sampling

**Q. What is a sample, why sample?** A subset of the population, used because we usually cannot measure everyone.
**Q. SRS vs stratified?** SRS: everyone equal chance. Stratified: divide into groups (strata), sample from each. I used hospitals as strata and took the same fraction from each, so all hospitals are represented.
**Q. When is stratified better?** When groups differ from each other, because it reduces the chance of an unbalanced sample.
**Q. Which is better here?** Stratified, slightly, since hospital means differ. Both gave means close to 3.976 anyway.
**Q. Sampling bias?** Systematic error from how people are picked, e.g. only the satisfied patients answer (non-response bias).

## C. Sampling distribution and CLT

**Q. What is the sampling distribution of the mean?** The distribution of x̄ over all possible samples of size n.
**Q. Standard error?** SD of the sampling distribution = σ/√n (estimated by s/√n). It measures how much sample means vary.
**Q. State the CLT.** For large n, the sampling distribution of the mean is approximately normal with mean μ and SD σ/√n, regardless of the population's shape.
**Q. Why does it matter?** It lets us use normal / t based CIs and tests even when the data are not exactly normal.
**Q. How did you show it?** 2000 samples of size 30: histogram of means is bell shaped and its SD (0.0948) ≈ s/√30 (0.0946).
**Q. SE vs standard deviation?** SD is the spread of individual values; SE is the spread of the sample mean. SE gets smaller with n, SD does not.

## D. Confidence interval

**Q. What is a 95% CI?** A range, computed from the sample, that would contain the true mean in about 95% of repeated samples.
**Q. Is "95% probability that μ is inside" correct?** No: μ is fixed, the interval is what varies. Right wording: "we are 95% confident".
**Q. Why t and not z?** σ is unknown so we use s, which adds uncertainty; the t distribution (df = n−1) has heavier tails to account for it. With n = 120 they are almost the same (1.98 vs 1.96).
**Q. Your overall CI?** (3.882, 4.069) around mean 3.976.
**Q. What makes a CI narrower?** Larger n, smaller s, lower confidence level.
**Q. Why does 99% give a wider interval?** To be more certain of capturing μ you need a wider net. Width 0.157 (90%), 0.187 (95%), 0.248 (99%).
**Q. CI for proportion?** p̂ ± z√(p̂(1−p̂)/n). Here 61/120 = 0.508, CI (0.419, 0.598).
**Q. Can you decide significance from CIs?** If CIs don't overlap, yes the means differ. Overlap is inconclusive. Hospital B and C barely touch but the t-test is significant, so use a test.
**Q. How does a CI relate to a hypothesis test?** A 95% CI contains exactly the values μ0 that a two-sided 5% test would not reject. Since 4.0 is inside (3.88, 4.07), we do not reject μ = 4.0.

## E. Hypothesis testing

**Q. Define H0 and H1.** H0 = no effect/difference (status quo), H1 = the effect we look for.
**Q. What is a p-value?** The probability of getting a result at least as extreme as the observed one if H0 is true.
**Q. What if p = 0.0013?** Very unlikely under H0, so reject H0.
**Q. Is p the probability that H0 is true?** No.
**Q. What is α?** The threshold for rejecting; equals the Type I error probability. I used 0.05.
**Q. Type I vs Type II error?** Type I: reject a true H0 (false positive), probability α. Type II: fail to reject a false H0 (false negative), probability β. Power = 1 − β.
**Q. Why "fail to reject" and not "accept"?** Not finding evidence against H0 is not proof that H0 is true (the sample may be too small).
**Q. One-tailed vs two-tailed?** One-tailed tests a direction (<, >), two-tailed tests any difference (≠). I used two-tailed because we only asked "is there a difference".
**Q. Z-test vs t-test?** Z: σ known or large n. t: σ unknown, uses s. n = 120 here, so both gave the same answer (−0.511, p ≈ 0.61).
**Q. Where did 4.0 come from?** It is an assumed target for illustration; the dataset has no given benchmark.
**Q. Independent vs paired t-test?** Independent: two separate groups. Paired: the same subjects measured twice. Here every patient has one score so paired does not apply.
**Q. Pooled vs Welch?** Pooled assumes equal variances; Welch does not. Levene said variances were similar, so pooled is fine, and Welch gave the same conclusion.
**Q. Statistical vs practical significance?** Significant means unlikely to be chance; practical means large enough to matter. B vs C differ by 0.37 on a 5-point scale which is modest but meaningful.

## F. ANOVA

**Q. Why ANOVA instead of several t-tests?** Multiple t-tests increase the overall chance of a false positive (3 tests about 14%, 6 tests about 26%). ANOVA tests all means in one go at 5%.
**Q. Hypotheses?** H0: all means equal; H1: at least one differs.
**Q. What is F?** F = MSB/MSW: variation between group means divided by variation within groups. Large F means group means differ more than noise explains.
**Q. What does F ≈ 1 mean?** Between-group variation is just like random noise, no evidence of a difference. (Age: F = 0.49.)
**Q. Degrees of freedom?** Between: k−1 (hospital 2, age 3). Within: N−k (hospital 117, age 116).
**Q. Is ANOVA one-tailed or two-tailed?** The F test is right-tailed (only large F is evidence against H0).
**Q. Assumptions?** Independence, normality within groups, equal variances.
**Q. How did you check them?** Shapiro-Wilk for normality (all p > 0.05), Levene for equal variances (p = 0.76 and 0.62).
**Q. What if assumptions fail?** Use Welch's ANOVA or non-parametric Kruskal-Wallis.
**Q. What does ANOVA not tell you?** Which groups differ, so a post-hoc test is needed.
**Q. Why "one-way"?** One factor (hospital or age group) is considered at a time. Two factors at once would be two-way ANOVA.
**Q. What is η²?** SSB/SST, the share of total variation explained by the groups. Hospital 0.094, age 0.013.
**Q. Relationship between ANOVA and t-test?** With 2 groups, F = t² and both give the same p-value.
**Q. Did you verify your ANOVA calculation?** Yes: my manual SSB/SSW calculation matches `scipy.stats.f_oneway` (F = 6.0759, p = 0.00309).

## G. Tukey HSD

**Q. Why Tukey?** To find which pairs of hospitals differ while keeping the total error rate at 5%.
**Q. What did it find?** Only B vs C (adjusted p = 0.0023, CI −0.624 to −0.115). A vs B p = 0.090, A vs C p = 0.509.
**Q. Why not run Tukey for age?** ANOVA was not significant, so there is nothing to follow up.
**Q. Why is A vs B significant in a simple t-test (p = 0.041) but not in Tukey?** Tukey adjusts for making 3 comparisons, so it is stricter. This is the point of a post-hoc test.

## H. Tricky "why" questions

**Q. Hospital B has the lowest mean, so B is the worst hospital?** Only B vs C is significant; B vs A is not. And the data is observational, so we cannot say why. Also patient mix may differ.
**Q. Age groups have different means (3.91 to 4.08). Why say no difference?** Differences of that size are what random sampling produces; ANOVA p = 0.687 says they're not significant.
**Q. If you reject H0, are you sure the means differ?** No, there is still a 5% chance of a Type I error.
**Q. What if you used α = 0.01?** Hospital ANOVA (p = 0.0031) would still be significant; B vs C too (0.0023).
**Q. Would a bigger sample change things?** Narrower CIs and more power; a real small difference (e.g. A vs B) might then become significant.
**Q. Could you use a Z-test for the B vs C comparison?** Z needs known σ or large samples; our groups are 42 and 44 and σ is unknown, so t is the right choice.
**Q. Is the rating scale truly continuous?** Strictly it is ordinal/discrete (rounded to 0.1), but with a mean near 4 and enough values it behaves like continuous data, and the assumption checks passed.

## I. Formula sheet

| Item | Formula |
|---|---|
| Sample mean | x̄ = Σx / n |
| Sample SD | s = √[Σ(x − x̄)² / (n − 1)] |
| Standard error | SE = s/√n |
| CI for mean (σ unknown) | x̄ ± t(α/2, n−1) · s/√n |
| CI for proportion | p̂ ± z · √(p̂(1−p̂)/n) |
| One-sample z / t | (x̄ − μ0) / (s/√n) |
| Pooled two-sample t | (x̄1 − x̄2) / (sp √(1/n1 + 1/n2)), sp² = [(n1−1)s1² + (n2−1)s2²]/(n1+n2−2) |
| SSB | Σ n_i (x̄_i − x̄)² |
| SSW | Σ Σ (x_ij − x̄_i)² |
| MSB, MSW | SSB/(k−1), SSW/(N−k) |
| F | MSB / MSW |
| η² | SSB / (SSB + SSW) |

**Critical values to remember:** z(95% two-sided) = 1.96; z(99%) = 2.576; z(90%) = 1.645.
