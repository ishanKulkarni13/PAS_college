# 08. Regression Analysis

> **Phase III (branch `cp-phase-3`):** the three decisions below (heteroscedasticity kept short, multicollinearity dropped, outlier sensitivity kept short) and the shorter interpretation text in the notebook. The regression code, equations and all numbers are unchanged. Cell numbers updated.

Notebook cells 75-86. Report sections 6.1 to 6.4.

## Decisions on three caveats (Phase III): what was kept, what was removed, and why

These three were in the notebook as extra caveats. After the Phase III review the group decided as follows.

| Caveat | In the syllabus / requirement? | Decision | In the notebook now |
|---|---|---|---|
| Heteroscedasticity (residual spread grows with the prediction) | **Yes.** The syllabus lists "Regression Assumptions" and constant variance is one of them | **Keep, short** | One sentence under the simple regression: the residuals fan out, so the constant-variance assumption does not hold well |
| Multicollinearity (predictors correlated with each other) | **No.** Not named in the syllabus or the project requirement | **Drop** | Nothing. The 0.72 to 0.79 correlations are still visible in the correlation section |
| Outlier sensitivity (refit without the Rs. 35 lakh listing) | **No.** Only the IQR outlier rule is in the syllabus (EDA). "Limitations" is required, but not this analysis | **Keep the cell, short text** | The refit cell stays; the explanation is two sentences |

**Heteroscedasticity: why keep.** It is a named syllabus topic, the residual plot that shows it is already in the notebook, and one plain sentence is what a student would normally write when they look at that plot. What was cut: the extra sentence saying "roughly normal errors" also fails, and the long wording.

**Multicollinearity: why drop.** It is not in the course or the requirement. It affects how stable individual coefficients are, not the predictions, and the project goal ("predict outcomes for unseen data using a held-out test set") is about predictions. Discussing it made the regression text look more curated than the course asks for. It was removed from the notebook's regression comment, the correlation comment, the limitations and the future scope (VIF). The "dummy variable trap" explanation for dropping Kolkata is a different, simpler idea and stays.

**Outlier sensitivity: why keep the cell.** Without it the results table shows a test R-squared (0.32) higher than the training R-squared (0.15), which a teacher will notice, and this cell is the only thing that explains it (the Rs. 35 lakh listing sits in the training rows; without it train R-squared is 0.25). It also gives the "limitations" requirement a real number to point at. The long paragraph was cut to: the listing changes the line a lot (R-squared 0.15 to 0.25 and slope 49.5 to 46.1 without it), which is why train R-squared is below test R-squared and why predictions on raw rent have limits.

**If asked in the viva:** (a) "Is variance constant?" - no, residuals fan out. (b) "Multicollinearity?" - one line, see the section below. (c) "Why is test R-squared above train?" - one extreme listing in the training rows.

## What regression does

Regression fits an equation that describes (and predicts) a response variable from one or more predictors. Here the response is **Rent**. It is the step from "are Rent and Size related?" (correlation) to "how many rupees does each extra sq. ft. add, and how well can we predict rent?"

## Setup (cells 75-76)

- **Train/test split:** rows are shuffled with a fixed seed (`RandomState(42)`); 20% (949 rows) are the **test set**, 80% (3,797) the **training set**. Models are fitted on the training rows only and judged on both. A model that does well on rows it has *not* seen is trustworthy; one that only does well on its own training rows may be overfitting.
- sklearn is not used (not installed, not needed); the formulas are written with numpy.

### Evaluation metrics (syllabus)

| Metric | Formula | Meaning |
|---|---|---|
| MAE | mean of absolute errors | Average miss in rupees; easy to explain |
| MSE | mean of squared errors | Rupees squared; punishes big errors |
| RMSE | square root of MSE | Back in rupees; punishes big errors more than MAE |
| R-squared | 1 - SSE/SST | Share of the variation in Rent explained by the model; 1 = perfect, 0 = no better than always predicting the mean |
| Adjusted R-squared | 1 - (1 - R2)(n - 1)/(n - k - 1) | R-squared corrected for the number of predictors k |

## Model 1: Simple linear regression, Rent ~ Size (cells 77-82)

**Model:** Rent = b0 + b1 x Size. **Least squares** chooses b0 and b1 to minimise the sum of squared residuals:

b1 = sum (x - x-bar)(y - y-bar) / sum (x - x-bar)^2,  b0 = y-bar - b1 x x-bar

**Fitted equation (training rows, cell 78):**

**Rent = -12,678.18 + 49.46 x Size** (checked with `scipy.stats.linregress`: same values)

- Slope 49.46: each extra sq. ft. adds about Rs. 49 to the predicted monthly rent on average.
- Intercept -12,678: the predicted rent at size 0, which has no real meaning (no flat has size near 0). A small flat can even get a negative predicted rent, so the straight line is clearly not ideal.

**Scores:**

| | Train | Test |
|---|---|---|
| MAE | 28,822.63 | 27,807.47 |
| MSE | 5,640,305,319.94 | 2,723,663,779.96 |
| RMSE | 75,101.97 | 52,188.73 |
| R-squared | 0.1488 | 0.3172 |

**Why is test R-squared (0.32) higher than train (0.15)?** One listing, the Rs. 35,00,000 Bangalore flat (3 BHK, 2,500 sq. ft.), fell into the training rows. It is very far from the line, so it inflates the training errors and total variation. Without it (cell 81): train R-squared rises from 0.1488 to 0.2466 and the slope drops from 49.46 to 46.06. Lesson: **one extreme row can move R-squared a lot**, so single R-squared numbers should be read with care.

**Interpretation:** Size explains only about 15% to 32% of rent variation; the typical miss (MAE about Rs. 28,000) is large compared with a median rent of Rs. 16,000. Correlation 0.41 squared is 0.17, matching the 17% of variance explained on the full data.

### Regression assumptions (syllabus) and how our data does

| Assumption | Check | Result for Rent ~ Size |
|---|---|---|
| Linearity | scatter plot (cell 51/79) | Roughly upward but very noisy |
| Independence of errors | study design | Plausible |
| Constant variance (homoscedasticity) | residual plot (cell 80) | **Violated**: residuals fan out as predicted rent grows (this one sentence is kept in the notebook) |
| Normal errors | residual plot | Violated (many large positive residuals), but no longer mentioned in the notebook |
| No perfect multicollinearity (multiple models) | correlations among predictors | Size, BHK, Bathroom correlated 0.72 to 0.79, not perfect (no longer discussed in the notebook, see Decisions below) |


## Model 2: Multiple linear regression (cells 83-86)

**Predictors:** Size, BHK, Bathroom and City. City is categorical, so it becomes **dummy (0/1) columns**, one per city except the baseline. **Kolkata (the cheapest city) is the baseline**, so each city coefficient is the difference from Kolkata, other things equal. Dropping one category avoids the "dummy variable trap" (perfect multicollinearity). Coefficients are found with `np.linalg.lstsq`.

Model **(2)** is fitted on Rent in rupees.

### Coefficients (cell 84, fitted on training rows)

| Term | Rent model (Rs.) |
|---|---|
| Intercept | -42,272.38 |
| Size (per sq. ft.) | 38.28 |
| BHK (per extra) | 3,450.23 |
| Bathroom (per extra) | 12,034.65 |
| City: Bangalore | 1,765.15 |
| City: Chennai | -7,305.62 |
| City: Delhi | 12,700.67 |
| City: Hyderabad | -16,522.69 |
| City: Mumbai | 58,370.91 |

**The raw model gives odd signs:** Chennai and Hyderabad come out *cheaper* than Kolkata although their average rents are higher. That is a sign the straight-line model is distorted by the skew in Rent.

### Comparison of the two models (cell 85, rupee scale)

| Model | Train R-squared | Test R-squared | Test MAE (Rs.) | Test RMSE (Rs.) |
|---|---|---|---|---|
| Simple: Rent ~ Size | 0.1488 | 0.3172 | 27,807 | 52,189 |
| Multiple: Rent | 0.2806 | 0.5204 | 22,345 | 43,739 |

Multiple model adjusted R-squared (train): 0.2791.

**Reading it:**
- Judged on the test rows, the multiple model is clearly better than the simple model (MAE about 22,345 versus 27,807; RMSE about 43,739 versus 52,189; test R-squared 0.52 versus 0.32).
- The train R-squared values (0.15 and 0.28) look low because the Rs. 35 lakh listing dominates the total variation in the training rows; MAE and RMSE on the test rows are the safer comparison.


## Multicollinearity (removed from the notebook)

Size, BHK and Bathroom are strongly correlated (0.72 to 0.79), which makes their *individual* coefficients less stable (a change in one can shift the others) while predictions stay fine. The notebook no longer says this; see the decision below. If asked: "they are correlated with each other, so we do not over-read single coefficients; our goal is prediction on unseen rows, and that is judged by the test MAE/RMSE/R-squared". A VIF check could quantify it (not done).

## Caveats to state

- **Association, not causation:** the models describe patterns in these listings.
- **One split:** results come from a single train/test split; cross-validation would be steadier.
- **Extrapolation:** do not predict outside the data range (for example sizes above 8,000 sq. ft.).
- **Why not machine learning?** This is a statistics course; linear regression is what the syllabus specifies and it is easier to interpret.

## Prediction example (for a demo)

Rent = -42,272.38 + 38.28 x Size + 3,450.23 x BHK + 12,034.65 x Bathroom + city effect. A sense check: a 2 BHK, 1 bathroom, 800 sq. ft. flat in Kolkata (the baseline, so no city effect) gives -42,272.38 + 38.2756 x 800 + 3,450.23 x 2 + 12,034.65, which is about Rs. 7,283, close to Kolkata's median of Rs. 8,500. (This example was computed outside the notebook with the same coefficients; it is not a notebook cell.)
