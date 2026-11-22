# 05. Law of Large Numbers and Central Limit Theorem

> **Phase III (branch `cp-phase-3`):** cell numbers updated. The LLN code is simpler now: the `to_numeric`/`dropna` step, the empty-data `ValueError`s and the `isinstance` filter on the sample sizes were removed, and `reference_mean` is taken straight from `rent.mean()`. The numbers in the results table below are identical to before (same seed, same shuffle).

Notebook cells 53-63. This is the group seminar topic (LLN) plus the CLT, which supports the inference chapter.

## Law of Large Numbers (LLN), cells 53-59

**Statement:** as the number of independent observations grows, the sample mean gets closer to the true mean of the distribution (it becomes more stable).

**Our design:**
1. **Reference mean** = mean of all 4,746 listings = Rs. 34,993.45 (cell 54). We call it the "reference mean", *not* the population mean, because our data is a collected sample, not every rental property in India.
2. Shuffle all 4,746 rents once with `sample(frac=1, random_state=42)` (cell 56). Every listing is in the shuffled list exactly once.
3. For sizes 10, 25, 50, 100, 250, 500, 1000, 2000, 3000, 4000 take the **first n rents of the shuffled list**; each sample is a *subset of the next bigger one* (nested). The mean of each is computed with an expanding mean.
4. Compare each sample mean with the reference mean and plot (cell 58).

**Why a random shuffle instead of the first n rows of the file?** Rows are ordered by city (Kolkata, Mumbai, ...), so the first n rows would all be from one city. A random shuffle gives every listing the same chance of being in a sample.

**Why nested samples?** It matches how LLN is usually pictured: you keep adding observations to the same sample. A fresh independent sample for each size would also be valid but jumpier.

**Why 4,746 is not in the list:** a sample of the whole dataset would equal the reference mean exactly (difference 0), which shows nothing. The largest sample size is 4,000.

**Why a fixed seed (`random_state=42`):** the result is reproducible; sir sees the same table every time.

### Results (cell 56)

| Sample size | Sample mean (Rs.) | Difference from reference (Rs.) |
|---|---|---|
| 10 | 60,850.00 | +25,856.55 |
| 25 | 50,300.00 | +15,306.55 |
| 50 | 46,380.00 | +11,386.55 |
| 100 | 41,134.99 | +6,141.54 |
| 250 | 33,893.95 | -1,099.50 |
| 500 | 37,549.68 | +2,556.23 |
| 1000 | 34,495.06 | -498.39 |
| 2000 | 35,995.49 | +1,002.03 |
| 3000 | 35,042.86 | +49.41 |
| 4000 | 35,394.75 | +401.30 |

**Reading it:** small samples can be far off (n = 10 is 74% above the reference mean because a few big rents were drawn). From about n = 250 onward the mean stays within a few thousand rupees of the reference. The difference does not shrink at every step (500 is further off than 250); LLN is about the long-run tendency, not a step-by-step guarantee.

**Real-life decision link (seminar):** a landlord, a rental portal or a student looking for a flat who judges "typical rent" from 5 or 10 listings can be badly misled; with a few hundred listings the estimate is stable. Insurance companies and quality-control teams use the same idea.

**Careful wording:** this finite-data demonstration illustrates LLN; it does not prove that the listings represent every rental in India.

## Central Limit Theorem (CLT), cells 60-63

**Statement:** the means of many random samples of size n are approximately normal, with mean mu and standard error sigma/sqrt(n), even when the data are not normal (for large enough n).

**Our demo:** for n = 30 and n = 100, draw 1,000 random samples (`rent.sample(n, random_state=i)` for i = 0..999) and take each sample's mean.

| n | Mean of the 1,000 sample means | Std of the sample means | Formula s/sqrt(n) | Skewness of sample means |
|---|---|---|---|---|
| 30 | 35,196.22 | 12,748.17 | 14,260.21 | 2.64 |
| 100 | 35,206.23 | 7,274.49 | 7,810.64 | 1.74 |
| (raw rents) | 34,993.45 | 78,106.41 | | 21.41 |

**Reading it:**
- The sample means centre on the reference mean (about 35,200 vs 34,993).
- The spread falls from 12,748 to 7,274 as n goes 30 to 100: a factor of 1.75 (theory says sqrt(100/30) = 1.83).
- The skewness drops from 21 (raw) to 2.6 (n = 30) to 1.7 (n = 100): the distribution of the mean gets more symmetric.
- The CLT works *slowly* here because the rents have such heavy tails ("n >= 30" is a rule of thumb for mild skew). With our full n = 4,746 the normal approximation is reasonable, so the t-based CI and tests are fine.
- The simulated spread is a little below s/sqrt(n) because a sample of 30 or 100 rarely contains the giant listings.

## LLN vs CLT in one line each

- **LLN:** *where* the sample mean ends up (near the true mean) as n grows.
- **CLT:** *how* the sample mean is distributed around it (roughly normal, spread sigma/sqrt(n)).

## Sampling distribution and standard error

The **sampling distribution of the mean** is the distribution of x-bar over all possible samples of size n. Its standard deviation is the **standard error** SE = s/sqrt(n). A bigger n means a smaller SE, a narrower confidence interval and a more reliable estimate. For the full dataset SE = 78,106.41 / sqrt(4,746) = Rs. 1,133.76 (cell 66).
