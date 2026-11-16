# 1. The question, explained

> A healthcare organization has conducted a survey to study patient satisfaction across different hospitals and age groups. Using Python, analyze the survey data by selecting appropriate samples, estimating population parameters using confidence intervals, performing suitable hypothesis tests and one-way ANOVA wherever applicable, interpreting the statistical significance of the results using p-values, and preparing a report that justifies whether the observed differences are statistically significant and supports the conclusions with appropriate statistical evidence.

## In plain words

A hospital group asked patients to rate their satisfaction (1 to 5). We know which hospital (A, B, C) and which age group each patient belongs to. The real question is:

1. Are some hospitals really better than others, or is it just luck in who got surveyed?
2. Do younger and older patients really feel differently, or is it just luck?

"Really" has a precise meaning in statistics: **statistically significant**, i.e. the difference is too big to be explained by random sampling variation alone.

## Phrase by phrase

| Phrase in the question | What it means / what we did |
|---|---|
| **selecting appropriate samples** | Pick a subset of patients in a fair way. We did a *simple random sample* (everyone has an equal chance) and a *stratified sample* (same hospital proportions as the full data) of 40 patients each. |
| **estimating population parameters using confidence intervals** | The population is "all patients of these hospitals"; its mean μ is the parameter. Our sample only gives a point estimate x̄, so we give a range (95% CI) that likely contains μ. |
| **suitable hypothesis tests** | Formal yes/no decisions about a claim, e.g. "is mean satisfaction 4.0?" (one-sample Z/t) and "do hospitals B and C have different means?" (two-sample t). |
| **one-way ANOVA wherever applicable** | When comparing **more than two** group means (3 hospitals, 4 age groups) use ANOVA instead of many t-tests. "One-way" = one grouping factor at a time. |
| **interpreting the statistical significance using p-values** | p < 0.05 means "if there was really no difference, results this extreme would happen less than 5% of the time", so we reject H0. |
| **preparing a report that justifies...** | The Observations and Conclusion cells: state which differences are significant, with numbers (F, p, CI). |

## Choices the question leaves open (and what I chose)

These are things Sir may ask "why did you do this?":

| Choice | What we did | Why |
|---|---|---|
| Treating the 120 rows as the population for sampling | Draw samples of 40 from it | The dataset is all we have; this demonstrates SRS vs stratified and CLT. |
| Which CI | t-interval | σ is unknown, so use s and the t distribution. |
| Benchmark for the one-sample test | μ0 = 4.0 | The question gives no target, so this is an assumed target (stated clearly in the notebook). |
| Z and t both | Z with s in place of σ, plus t | Unit III slides teach both; n = 120 is large so they agree. |
| Paired t-test | Not used | Each patient has one score, no "before / after". Groups are independent. |
| Post-hoc | Tukey HSD | ANOVA only says "some difference"; Tukey says which pairs. |
| Proportion CI | "satisfied" = score ≥ 4 | The syllabus lists CI for proportion; it is a quick extra. |
