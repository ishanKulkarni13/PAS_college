# 08. Regression Analysis

Notebook cells 78-89. Report sections 6.1 to 6.4.

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


## Model 2: Multiple linear regression (cells 86-89)

**Predictors:** Size, BHK, Bathroom and City. City is categorical, so it becomes **dummy (0/1) columns**, one per city except the baseline. **Kolkata (the cheapest city) is the baseline**, so each city coefficient is the difference from Kolkata, other things equal. Dropping one category avoids the "dummy variable trap" (perfect multicollinearity). Coefficients are found with `np.linalg.lstsq`.

Model **(2)** is fitted on Rent in rupees.

### Coefficients (cell 87, fitted on training rows)

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

**The raw model gives odd signs:** Chennai and Hyderabad come out *cheaper* than Kolkata although their average rents are higher. That is a sign the straight-line model is distorted by the skew in Rent and by the correlation among the predictors.

### Comparison of the two models (cell 88, rupee scale)

| Model | Train R-squared | Test R-squared | Test MAE (Rs.) | Test RMSE (Rs.) |
|---|---|---|---|---|
| Simple: Rent ~ Size | 0.1488 | 0.3172 | 27,807 | 52,189 |
| Multiple: Rent | 0.2806 | 0.5204 | 22,345 | 43,739 |

Multiple model adjusted R-squared (train): 0.2791.

**Reading it:**
- Judged on the test rows, the multiple model is clearly better than the simple model (MAE about 22,345 versus 27,807; RMSE about 43,739 versus 52,189; test R-squared 0.52 versus 0.32).
- The train R-squared values (0.15 and 0.28) look low because the Rs. 35 lakh listing dominates the total variation in the training rows; MAE and RMSE on the test rows are the safer comparison.


## Multicollinearity

Size, BHK and Bathroom are strongly correlated (0.72 to 0.79). Predictions are still fine, but the *individual* coefficients of these three are less stable (a change in one can shift the others). Example: in the raw model BHK gets only +3,450 per extra BHK because Size and Bathroom already carry much of the same information. A VIF check could quantify this (not done).

## Caveats to state

- **Association, not causation:** the models describe patterns in these listings.
- **One split:** results come from a single train/test split; cross-validation would be steadier.
- **Extrapolation:** do not predict outside the data range (for example sizes above 8,000 sq. ft.).
- **Why not machine learning?** This is a statistics course; linear regression is what the syllabus specifies and it is easier to interpret.

## Prediction example (for a demo)

Rent = -42,272.38 + 38.28 x Size + 3,450.23 x BHK + 12,034.65 x Bathroom + city effect. A sense check: a 2 BHK, 1 bathroom, 800 sq. ft. flat in Kolkata (the baseline, so no city effect) gives -42,272.38 + 38.2756 x 800 + 3,450.23 x 2 + 12,034.65, which is about Rs. 7,283, close to Kolkata's median of Rs. 8,500. (This example was computed outside the notebook with the same coefficients; it is not a notebook cell.)
