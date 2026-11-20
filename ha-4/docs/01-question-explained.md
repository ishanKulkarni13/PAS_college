# 1. The question, explained

> A university intends to predict students' final examination performance using attributes such as attendance, study hours, assignment scores, and internal assessment marks. Using Python and suitable machine learning libraries, preprocess the dataset, perform exploratory data analysis, identify relationships among variables through correlation analysis, develop a simple linear regression model for predicting final examination marks, evaluate the model using appropriate regression performance metrics, visualize the prediction results, interpret the findings, and present the complete analysis through a concise technical report.

## In plain words

The university has data for 100 students: how much they attended, how many hours they study, assignment marks, internal marks, and the final exam marks they got. The task is to build a small machine learning model that, given a student's earlier numbers, **predicts the final exam marks**, and to check how good that prediction is.

This is **supervised learning, regression**:
- **Features / predictors (X)**: attendance, study hours, assignment marks, internal marks.
- **Target (y)**: final exam marks (a number, so it is regression and not classification).

## Phrase by phrase

| Phrase | What it means / what we did |
|---|---|
| **suitable machine learning libraries** | scikit-learn (`LinearRegression`, `train_test_split`, metrics), plus pandas / numpy / matplotlib / scipy. |
| **preprocess the dataset** | Check missing values and duplicates (none), drop the `Student_ID` column (just a label), make sure columns are numeric. |
| **exploratory data analysis** | Describe the data (mean, spread), histograms, scatter plots of each factor against final marks. |
| **identify relationships through correlation analysis** | Pearson correlation matrix and heatmap; see which factor moves most with the final marks. |
| **simple linear regression** | **One** predictor only: y = b0 + b1·x. We pick the best one (Internal_Marks) from the correlation. (Using several predictors would be *multiple* regression.) |
| **evaluate using regression performance metrics** | MAE, MSE, RMSE, R² on training and test data. |
| **visualize the prediction results** | Regression line over the data, actual vs predicted plot, residual plots. |
| **interpret the findings** | Say what the slope means, how accurate the model is, what R² says, limits. |
| **concise technical report** | The Observations and Conclusion cells. |

## Choices the question leaves open (and what I chose)

| Choice | What we did | Why |
|---|---|---|
| Which predictor for the *simple* regression | Internal_Marks | highest correlation with final marks (r = 0.60) |
| Train/test split | 80/20, `random_state = 42` | standard; test data shows performance on unseen students |
| Scaling | none | simple linear regression does not need it, original units keep the slope easy to read |
| Extra: other predictors, cross-validation, multiple regression | small comparison at the end | 20 test students is a very small test set, and it answers "why only one predictor?" in the viva |
| Residual check | plots + Shapiro-Wilk | the assumptions of linear regression should be justified |
