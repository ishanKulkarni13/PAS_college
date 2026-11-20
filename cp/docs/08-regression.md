# 08. Regression Analysis

Notebook cells 78-90. Report sections 6.1 to 6.4.

## What regression does

Regression fits an equation that describes (and predicts) a response variable from one or more predictors. Here the response is **Rent**. It is the step from "are Rent and Size related?" (correlation) to "how many rupees does each extra sq. ft. add, and how well can we predict rent?"

## Setup (cells 78-79)

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

## Model 1: Simple linear regression, Rent ~ Size (cells 80-85)

**Model:** Rent = b0 + b1 x Size. **Least squares** chooses b0 and b1 to minimise the sum of squared residuals:

b1 = sum (x - x-bar)(y - y-bar) / sum (x - x-bar)^2,  b0 = y-bar - b1 x x-bar

**Fitted equation (training rows, cell 81):**

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

**Why is test R-squared (0.32) higher than train (0.15)?** One listing, the Rs. 35,00,000 Bangalore flat (3 BHK, 2,500 sq. ft.), fell into the training rows. It is very far from the line, so it inflates the training errors and total variation. Without it (cell 84): train R-squared rises from 0.1488 to 0.2466 and the slope drops from 49.46 to 46.06. Lesson: **one extreme row can move R-squared a lot**, so single R-squared numbers should be read with care.

**Interpretation:** Size explains only about 15% to 32% of rent variation; the typical miss (MAE about Rs. 28,000) is large compared with a median rent of Rs. 16,000. Correlation 0.41 squared is 0.17, matching the 17% of variance explained on the full data.

### Regression assumptions (syllabus) and how our data does

| Assumption | Check | Result for Rent ~ Size |
|---|---|---|
| Linearity | scatter plot (cell 51/82) | Roughly upward but very noisy |
| Independence of errors | study design | Plausible |
| Constant variance (homoscedasticity) | residual plot (cell 83) | **Violated**: residuals fan out as predicted rent grows |
| Normal errors | residual plot | **Violated**: many large positive residuals |
| No perfect multicollinearity (multiple models) | correlations among predictors | Size, BHK, Bathroom correlated 0.72 to 0.79, not perfect |

The two violated assumptions are exactly what a log transform of Rent is meant to repair.

## Model 2 and 3: Multiple linear regression (cells 86-90)

**Predictors:** Size, BHK, Bathroom and City. City is categorical, so it becomes **dummy (0/1) columns**, one per city except the baseline. **Kolkata (the cheapest city) is the baseline**, so each city coefficient is the difference from Kolkata, other things equal. Dropping one category avoids the "dummy variable trap" (perfect multicollinearity). Coefficients are found with `np.linalg.lstsq`.

Two versions: **(2) on Rent** in rupees and **(3) on log(Rent)**.

### Coefficients (cell 87, fitted on training rows)

| Term | Rent model (Rs.) | log(Rent) model | log model: approx. % change in rent |
|---|---|---|---|
| Intercept | -42,272.38 | 8.0499 | |
| Size (per sq. ft.) | 38.28 | 0.0005 | +0.05% per sq. ft. (about 5% per 100 sq. ft.) |
| BHK (per extra) | 3,450.23 | 0.2307 | +25.9% |
| Bathroom (per extra) | 12,034.65 | 0.1821 | +20.0% |
| City: Bangalore | 1,765.15 | 0.3200 | +37.7% vs Kolkata |
| City: Chennai | -7,305.62 | 0.2246 | +25.2% |
| City: Delhi | 12,700.67 | 0.5826 | +79.1% |
| City: Hyderabad | -16,522.69 | 0.0874 | +9.1% |
| City: Mumbai | 58,370.91 | 1.5372 | +365.1% |

**How to read the log model:** if ln(Rent) = a + b x, then a one-unit increase in x multiplies rent by e^b, so the percentage change is (e^b - 1) x 100. Example: Mumbai b = 1.5372, e^1.5372 = 4.65, so rent is about 365% higher than in Kolkata for a comparable flat.

**The raw model gives odd signs:** Chennai and Hyderabad come out *cheaper* than Kolkata although their average rents are higher. That is a sign the raw straight-line model is distorted by skew. In the log model all city effects are positive and follow the order of the city averages (Mumbai > Delhi > Bangalore > Chennai > Hyderabad > Kolkata).

### Comparison of the three models (cell 88, rupee scale)

| Model | Train R-squared | Test R-squared | Test MAE (Rs.) | Test RMSE (Rs.) |
|---|---|---|---|---|
| Simple: Rent ~ Size | 0.1488 | 0.3172 | 27,807 | 52,189 |
| Multiple: Rent | 0.2806 | 0.5204 | 22,345 | 43,739 |
| Multiple: log(Rent), converted back to Rs. | 0.2812 | 0.7486 | 12,230 | 31,667 |

Log model on the log scale: **train R-squared 0.7738, adjusted R-squared 0.7734, test R-squared 0.7913**. Raw multiple model adjusted R-squared (train) 0.2791.

**Reading it:**
- Judged on the test rows, the multiple model on log(Rent) is clearly best (MAE about 12,230 versus 22,345 and 27,807).
- On the log scale about three quarters of the variation is explained (0.77 train, 0.79 test), and train is close to test, so there is no sign of overfitting.
- The rupee-scale train R-squared values (0.15 to 0.28) look low because the Rs. 35 lakh listing dominates the total variation in the training rows; MAE and RMSE on the test rows are the safer comparison. Do not compare R-squared across scales without saying which scale.

### Plots (cell 89)
- **Actual vs predicted (left):** points near the red diagonal are well predicted; the model under-predicts the very expensive listings.
- **Residuals on log scale (right):** spread evenly around zero with no fan shape, so constant variance is reasonable for the log model.

## Multicollinearity

Size, BHK and Bathroom are strongly correlated (0.72 to 0.79). Predictions are still fine, but the *individual* coefficients of these three are less stable (a change in one can shift the others). Example: in the raw model BHK gets only +3,450 per extra BHK because Size and Bathroom already carry much of the same information. A VIF check could quantify this (not done).

## Caveats to state

- **Back-transformation:** exp(predicted log rent) estimates a typical (median-like) rent, not the mean; it slightly under-predicts the average. We did not apply a correction.
- **Association, not causation:** the models describe patterns in these listings.
- **One split:** results come from a single train/test split; cross-validation would be steadier.
- **Extrapolation:** do not predict outside the data range (for example sizes above 8,000 sq. ft.).
- **Why not machine learning?** This is a statistics course; linear regression is what the syllabus specifies and it is easier to interpret.

## Prediction example (for a demo, from the log model)

For a predicted rent use ln(Rent) = 8.0499 + 0.0005 x Size + 0.2307 x BHK + 0.1821 x Bathroom + city effect, then Rent = e^(that). A sense check: a 2 BHK, 1 bathroom, 800 sq. ft. flat in Kolkata gives ln(Rent) = 8.0499 + 0.000497 x 800 + 0.2307 x 2 + 0.1821 = 9.0913 (the Size coefficient 0.0005 in the table is rounded; its exact value is 0.000497), so Rent = e^9.0913, which is about Rs. 8,877, close to Kolkata's median of Rs. 8,500. (This example was computed outside the notebook with the same coefficients; it is not a notebook cell.)
