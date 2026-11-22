# 13. Phase III: What Changed from `main` to `cp-phase-3`

This file only records changes. It does not explain the project (see the other docs for that).

| | |
|---|---|
| Branch | `cp-phase-3` (git does not allow spaces in branch names, so it is not "cp phase-3") |
| Branched from | `main` at commit `a893f8c` ("remove log-transformation analysis from regression notebook and docs") |
| Files changed | `cp/property_location_rental_price_analysis.ipynb`, 14 files in `cp/docs/`, and this new file |
| Not touched | `cp/data`, `cp/reference`, `cp/random-not-imp`, `cp/docs/report/`, everything outside `cp/` |
| Notebook size | 92 cells on `main`, 89 cells on `cp-phase-3` |

## 1. Reasons, in short

The review of the notebook (by Codex, then the group) said it looked more elaborate than a student project needs, and that it should stay simple and cover what the course asks. The group's decisions:

| Review point | Decision |
|---|---|
| "Reference mean" instead of population mean | Keep |
| Exact CI interpretation (95% of repeated intervals...) | Tone down to plain wording |
| Equal-variance concerns | Remove |
| Heteroscedasticity | Keep, short (it is in the syllabus: regression assumptions) |
| Multicollinearity | Drop (not in the syllabus or the requirement) |
| Outlier sensitivity analysis | Keep the cell, short text (it explains test R-squared > train R-squared) |
| Defensive code (empty checks, type checks, strength-labelling loop) | Remove; keep normal cleaning checks |
| Pearson and Spearman both | Keep Pearson only (requirement says "Pearson or Spearman") |
| Rewriting the prose in the group's own words | The group will do this |
| Showing anything besides the notebook | No, only the notebook is shown |

Why for the three regression caveats is written out in [08-regression.md](08-regression.md), section "Decisions on three caveats".

## 2. Notebook changes (`property_location_rental_price_analysis.ipynb`)

Cell numbers in this section: "old" = `main`, "new" = `cp-phase-3`.

### 2.1 Cells deleted (3)

| Old cells | Content |
|---|---|
| 53 | Markdown: "Spearman correlation" explanation |
| 54 | Code: Pearson vs Spearman comparison table |
| 55 | Markdown: comment on Spearman vs Pearson |

### 2.2 Cells rewritten

| Old cell | New cell | What changed |
|---|---|---|
| 1 | 1 | Objective changed from "(Pearson and Spearman)" to "(Pearson)" |
| 46 | 46 | Section intro shortened |
| 47 (code) | 47 | Was: coerce to numeric, raise error if empty, loop that classified every pair as negligible/weak/moderate/strong, a pair-results table. Now: `numeric_data = df[numeric_columns]`, then print `.cov()` and `.corr()`. Same numbers |
| 48 | 48 | Long note on strength bands replaced by three sentences with the actual correlation values |
| 50 | 50 | Heatmap comment reduced to one line |
| 52 | 52 | Scatter comment shortened; now also says the y-axis is cut at Rs. 5,00,000 and hides the largest listing |
| 56 | 53 | One sentence simplified (reference mean wording) |
| 57 (code) | 54 | Removed `to_numeric`/`dropna` and the empty-data `ValueError`. Now `reference_mean = rent.mean()` |
| 58 | 55 | Shortened |
| 59 (code) | 56 | Removed the `isinstance`/range filter on the sample sizes and the `ValueError`. Sample sizes are a plain list; shuffle uses `rent.sample(frac=1, random_state=42)`. Same table of results |
| 71 | 68 | CI interpretation: now "we are 95% confident that the mean rent lies in this range" instead of the repeated-sampling explanation and the "not a 95% chance" warning |
| 72 | 69 | Removed the sentence saying the pooled test "assumes the two population variances are equal" |
| 73 (code) | 70 | Comment "(equal variances assumed)" removed. Code unchanged |
| 74 | 71 | Removed the "equal population variances, not verified, SDs 2.3 times apart" bullet |
| 75 | 72 | ANOVA assumptions now "independent observations and roughly normal data in each group" (equal variances removed) |
| 77 | 74 | Removed the equal-variance paragraph; kept independence and normality (CLT, 524 to 972 per city) |
| 85 | 82 | Simple regression reading cut to four short paragraphs: equation and intercept; weak R-squared; the Rs. 35 lakh listing (R-squared 0.15 to 0.25 and slope 49.5 to 46.1 without it, why train < test, predictions on raw rent have limits); one sentence on heteroscedasticity (residuals fan out, constant variance does not hold well). The "roughly normal errors" sentence was removed |
| 89 | 86 | Removed the multicollinearity sentence ("Size, BHK and Bathroom are strongly correlated... coefficients not very stable"). The paragraph heading is now "Caution" |
| 90 | 87 | Key finding 5: removed "Both tests assume equal variances, which we have not verified." Finding 6: removed the Spearman values |
| 91 | 88 | Conclusion: removed "with both Pearson and Spearman", simplified the LLN sentence. Limitations: removed the long equal-variance bullet |

### 2.3 Cells not changed in content

Every other cell keeps its text and code. Their numbers: cells 0, 2-45, 49, 51 are the same; old 60-70, 76, 78-84, 86-88 became new 57-67, 73, 75-81, 83-85 (all shifted by minus 3).

### 2.4 Old-to-new cell number map

| Old | New |
|---|---|
| 0 to 52 | same |
| 53, 54, 55 | deleted |
| 56 to 91 | 53 to 88 (subtract 3) |

### 2.5 Kept on purpose

The reference-mean wording, the outlier sensitivity cell (new 81), the residual plot (new 80), the CLT section, the LLN section, the Furnished vs Unfurnished t-test (pooled, by hand), the ANOVA, simple and multiple regression with dummy variables, and the train/test evaluation.

### 2.6 Verification

The notebook was re-run from top to bottom after the code changes. It finishes without errors. The printed output of every code cell that was not edited is identical to the output on `main` (same numbers for the LLN table, CLT, CI, t-test, ANOVA, regression). The only code cells whose output changed are the correlation cell (new 47: same values, now two matrices instead of a pair table) and the reference-mean cell (new 54: wording of the printed labels). The text-only edits in 2.2 (the regression reading, the multiple-model reading) were made afterwards and do not change any output.

## 3. Docs changes (`cp/docs/`)

Each doc below also has a "Phase III" note near the top saying what changed in that file.

| File | Change |
|---|---|
| `README.md` | 89 cells; Spearman removed from the list of methods; row for this file added; status section updated |
| `methodology.md` | "(Pearson, Spearman)" changed to "(Pearson)" |
| `01-project-overview.md` | Cell numbers updated; key results 3 and 4 no longer mention equal variances or Spearman |
| `02-template-coverage.md` | Whole table renumbered to the 89-cell notebook; section 5.2 is Pearson only; "equal variances not verified" removed; cell count 89 |
| `03-dataset-sampling-cleaning.md` | Cell numbers updated; "median and Spearman" changed to "median" |
| `04-descriptive-and-distribution.md` | Observation 4 no longer mentions Spearman or equal variances |
| `05-lln-and-clt.md` | Cell numbers updated; note that the defensive LLN code was removed (results unchanged) |
| `06-inference.md` | CI interpretation now has the simple wording first and the formal wording as "if sir pushes"; equal-variance caveats removed from the t-test and ANOVA sections |
| `07-correlation.md` | Spearman sections replaced by a Pearson table and a short "Spearman (removed in Phase III)" section that keeps the old numbers for the viva; multicollinearity sentence removed from the interpretation |
| `08-regression.md` | New section "Decisions on three caveats (Phase III)" with the keep/drop reasons; assumption table, "odd signs" sentence and multicollinearity section updated |
| `09-results-summary.md` | Spearman table replaced by a Pearson table; equal-variance remarks removed; conclusion paragraph said "three hypothesis tests" but there are two: fixed |
| `10-notebook-walkthrough.md` | Rewritten for the 89-cell notebook; list of variables that no longer exist |
| `11-formulas-cheatsheet.md` | Spearman row marked "syllabus topic, not used in the notebook"; equal-variance remark shortened |
| `12-limitations-and-future-scope.md` | Equal-variance and multicollinearity limitations removed; future scope items "formal variance check" and "VIF" removed |
| `VIVA.md` | Q41, Q49, Q50, Q51 (now "Why not Welch's t-test?"), Q59, Q68, Q69, Q71, Q79, Q81, Q88 and Q93 rewritten to match |
| `13-phase-3-changes-from-main.md` | This file (new) |

## 4. Things to remember before the showcase

- Run the notebook from the `PAS_college` kernel with Run All shortly before presenting.
- The notebook no longer contains Spearman, equal-variance remarks or multicollinearity, but sir can still ask about them. Answers are in `VIVA.md` (Q51, Q59, Q68, Q69, Q88).
- The histogram of rent is cut at Rs. 3,00,000 and the scatter and regression plots at Rs. 5,00,000; say so when showing them (the scatter comment in the notebook already says it).
- If any of the removed items should come back, the `main` branch still has the old notebook and docs.
