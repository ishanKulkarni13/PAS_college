# 3. Code walkthrough: `ha-3.ipynb`

Be able to explain every block below in one or two sentences.

## Setup
| Block | What it does | Why |
|---|---|---|
| imports: pandas, numpy, matplotlib, `scipy.stats` | tables, maths, plots, statistical tests | `scipy.stats` has t, norm, f distributions and the tests (`ttest_1samp`, `ttest_ind`, `levene`, `shapiro`, `f_oneway`, `tukey_hsd`) |
| `pd.read_csv(...)`, `df.head()` | load the CSV, look at the first rows | first sanity check of columns |
| `df.shape`, `df.info()` | 120 rows x 4 columns, data types | confirms numeric score, text categories |
| `isnull().sum()`, `duplicated().sum()` | missing values and duplicate rows (both 0) | cleaning check, nothing to fix |
| `value_counts()` on Hospital / Age_Group | group sizes: A 34, B 42, C 44; 18-30: 30, 31-45: 26, 46-60: 27, 60+: 37 | group sizes are uneven, good to know before ANOVA |

## Descriptive statistics
- `score = df['Satisfaction_Score']`: shortcut for the column.
- mean 3.9758, median 4.0, mode 4.0, std 0.5181, min 2.4, max 5.0. Mean ≈ median so the distribution is fairly symmetric.
- `groupby(...)['Satisfaction_Score'].agg([...])`: count / mean / median / std per group. This already hints B is lowest.

## Sampling
- `df.sample(n=40, random_state=7)`: simple random sample of 40. `random_state` makes it repeatable.
- `df.groupby('Hospital').sample(frac=40/120)`: takes the same fraction (1/3) from each hospital, i.e. a **proportionate stratified sample** (A: 11, B: 14, C: 15).
- Compare sample means (3.95, 3.9575) with the full mean (3.9758): all close.

## Sampling distribution / CLT
- Loop of 2000: `score.sample(n=30, replace=True, random_state=i).mean()`, collects 2000 sample means.
- `np.mean(means30)` ≈ population mean; `np.std(means30)` ≈ `score.std()/np.sqrt(30)` (0.0948 vs 0.0946): the standard error formula works.
- Histogram is bell shaped: CLT. The red dashed line is the population mean.

## Confidence intervals
- `mean_ci(x, conf)`: my own function:
  1. `se = x.std()/np.sqrt(n)` (pandas `std` uses n−1, which is the sample sd)
  2. `tcrit = stats.t.ppf((1+conf)/2, n-1)`: for 95%, the 97.5th percentile of t
  3. returns `xbar ± tcrit*se`
- `stats.t.interval(...)` with `stats.sem`: scipy's own version as a cross-check. Both give (3.8822, 4.0695).
- Loops over `groupby` to build `hosp_ci` and `age_ci` tables.
- 90 / 95 / 99%: widths 0.157 / 0.187 / 0.248. Higher confidence = wider.
- Proportion CI: `satisfied = score >= 4` gives a True/False column; its mean is p̂ = 0.5083 (61 of 120); `se_p = sqrt(p̂(1−p̂)/n)`; `z95 = norm.ppf(0.975)`.

## Hypothesis tests
- **One-sample**: `z = (xbar − mu0)/(s/√n)`, `p_z = 2*(1 − norm.cdf(|z|))` (two tailed). `stats.ttest_1samp(score, 4.0)` does the t version. Both: statistic −0.511, p ≈ 0.61.
- **B vs C**: `stats.levene(b, c)` first (p = 0.70, equal variances OK), `stats.ttest_ind(b, c, equal_var=True)` is the pooled t-test (t = −3.33, p = 0.0013). `equal_var=False` is Welch (same decision).
- **18-30 vs 60+**: same steps, p = 0.61.
- Paired t-test: written as a note only, with the reason it does not apply.

## ANOVA
- `my_anova(groups)`: manual calculation:
  - `grand` = mean of all scores; `ssb` = Σ n_i (mean_i − grand)²; `ssw` = Σ Σ (x − mean_i)²
  - `msb = ssb/(k−1)`, `msw = ssw/(N−k)`, `F = msb/msw`
  - `p = 1 − stats.f.cdf(F, k−1, N−k)`; `eta2 = ssb/(ssb+ssw)`
- `by_hosp`, `by_age`: lists of NumPy arrays, one per group (what `f_oneway` expects).
- `stats.f_oneway(*by_hosp)`: the `*` unpacks the list into separate arguments. Same F and p as my manual version (check!).
- Assumptions: `stats.levene(*groups)` (equal variance), `stats.shapiro(group)` for each group (normality).
- `stats.tukey_hsd(*by_hosp)`: returns a matrix of p-values and CIs for every pair; the loop prints them with hospital names (scipy only numbers the groups 0, 1, 2 = A, B, C, alphabetical order).

## Plots
- `plt.boxplot(groups, tick_labels=...)`: box = Q1 to Q3, line = median, whiskers = typical range, dots = outliers. B sits lowest.
- `plt.errorbar(...)` with `yerr` = half-width of the CI: mean with 95% CI for each hospital and age group. Dashed line = overall mean.

## Things you can say if asked "what would you change?"
- Use a real target from the hospital instead of the assumed 4.0.
- Collect more data per group; sizes are uneven (26 to 44).
- Check whether patient mix differs between hospitals (more young patients in one hospital could confound the result). We only tested one factor at a time (one-way); a two-way ANOVA could test hospital and age together.
