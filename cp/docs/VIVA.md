# VIVA Questions and Answers

Grouped by topic. Numbers are from the executed notebook (see [09-results-summary.md](09-results-summary.md)). The questions marked **(tricky)** are the ones most likely to catch someone out.

---

## A. Project and data

**Q1. What is the objective of your project?**
To study how monthly rent varies with city and property characteristics (size, BHK, bathrooms, furnishing), and to apply descriptive statistics, confidence intervals, hypothesis tests, correlation and regression. We also demonstrate the Law of Large Numbers (our seminar topic) on the same data.

**Q2. Why did you choose this dataset?**
It is a real dataset that matches the syllabus project area "House Price Analysis and Prediction System". It has a numerical main variable (Rent), a location variable (City) and several numerical predictors (Size, BHK, Bathroom), so every course topic can be applied. It has no missing values or duplicates.

**Q3. Describe the dataset.**
4,746 rental listings, 12 columns, six cities (Mumbai 972, Chennai 891, Bangalore 886, Hyderabad 868, Delhi 605, Kolkata 524), posted between April and July 2022. Rent ranges from Rs. 1,200 to Rs. 35,00,000.

**Q4. Which variables did you use and what type are they?**
Rent (numerical, continuous, main variable), Size (numerical, continuous), BHK and Bathroom (numerical, discrete), City (categorical, location variable), Furnishing Status (categorical, used in one t-test). We did not use Floor, Area Type, Area Locality, Tenant Preferred, Point of Contact or Posted On.

**Q5. Why City and not Area Locality as the location variable?**
There are 2,235 different localities for 4,746 rows, about two listings per locality, so groups would be too small to compare. City gives six large groups.

**Q6. How was the data collected? What sampling method?**
The author does not describe a random sampling plan, so we treat the data as one non-probability (convenience) sample of the rental market. It is not a simple random sample and not a census, so our results describe these listings and are a guide to the wider market.

**Q7. What is the sample size and is it enough?** (tricky)
n = 4,746, which is large. With a sample this big the sample mean is well behaved (CLT), so CIs and t-tests are reasonable even though rent is skewed. A large sample does not remove bias, though: if the listings are not representative, more of them does not fix that.

**Q8. What are the possible sources of bias?**
Only listed properties, only six big cities, one three-month window, listed (asking) rents rather than final rents, and owners/agents decide what to post. Mumbai is over-represented compared with Kolkata.

**Q9. Did you clean the data? Did you remove outliers?**
We checked missing values (none), duplicates (none) and non-positive values (none). We found 16 listings with size below 50 sq. ft. and 520 listings above the IQR upper bound (Rs. 67,500). We removed nothing, because we cannot prove they are errors. Instead we show their effect and use methods that cope with them.

**Q10. Why did you not remove the Rs. 35 lakh listing?** (tricky)
It is a 3 BHK of 2,500 sq. ft. in Bangalore, where the median rent is Rs. 14,000, so it may be a data-entry error, but we cannot confirm that. Deleting it would change the results by our choice. We instead showed what it does: for the simple regression, R-squared on the training rows goes from 0.15 with it to 0.25 without it. A good next step is to check it against the source.

**Q11. Why did you not use machine learning?**
It is a statistics course project. The syllabus asks for descriptive statistics, inference, correlation and linear regression. Linear regression is also easier to interpret than a black-box model.

---

## B. Descriptive statistics

**Q12. Define mean, median and mode. What are their values for Rent?**
Mean: sum divided by count, Rs. 34,993. Median: middle value when sorted, Rs. 16,000. Mode: most frequent value, Rs. 15,000.

**Q13. Why is the mean more than twice the median?** (tricky)
Rent is right-skewed: a few very expensive listings pull the mean up but hardly move the median. The median describes a typical listing better.

**Q14. Which measure of central tendency would you report and why?**
The median for a "typical" rent because of the skew, and the mean when we need it for the CI, t-tests and LLN, which are built on means.

**Q15. What are variance and standard deviation? Which formula did you use?**
Variance is the average squared deviation from the mean; standard deviation is its square root, in rupees. We use the sample formulas (divide by n-1) because the data is a sample. Variance is about 6.1 billion rupees squared and SD is Rs. 78,106.

**Q16. Why divide by n-1?**
The mean is estimated from the same data, which makes the deviations slightly too small. Dividing by n-1 corrects this and gives an unbiased estimate of the population variance. With n = 4,746 the difference from dividing by n is tiny.

**Q17. What is IQR and what is its value?**
Q3 - Q1, the spread of the middle 50%. Q1 = Rs. 10,000, Q3 = Rs. 33,000, so IQR = Rs. 23,000. It is not affected by extreme values.

**Q18. What is range? Is it a good measure here?**
Maximum minus minimum: 35,00,000 - 1,200 = Rs. 34,98,800. It depends only on two values, and here one of them is an extreme, so it is a poor summary. IQR is better.

**Q19. How did you detect outliers?**
IQR rule: below Q1 - 1.5 IQR (-24,500) or above Q3 + 1.5 IQR (67,500). 520 listings (about 11%) are above the upper bound.

**Q20. What are skewness and kurtosis? What did you get?**
Skewness measures asymmetry (positive = long right tail); excess kurtosis measures tail heaviness (0 for normal). Rent: skewness 21.41, excess kurtosis 841.11, extremely skewed and heavy-tailed. log(Rent): 0.91 and 0.88.

**Q21. Why did you take the log of Rent?**
It compresses very large values, reducing skewness from 21 to 0.9, and it repairs the regression assumptions (constant variance, roughly normal errors) that failed for raw Rent.

**Q22. Is Rent normally distributed?**
No. Skewness of 21 and kurtosis of 841 make that obvious; the histogram has a long right tail.

**Q23. Which city is the most expensive and which the cheapest?**
Mumbai (mean Rs. 85,321, median Rs. 52,000) and Kolkata (mean Rs. 11,645, median Rs. 8,500).

**Q24. Bangalore has a mean of only Rs. 24,966 but the largest standard deviation. Why?** (tricky)
One listing of Rs. 35,00,000 inflates Bangalore's SD (120,056) and widens its confidence interval. Bangalore's median is a modest Rs. 14,000, the same as Chennai and Hyderabad.

**Q25. How do you read a box plot? Why a log axis?**
Box = Q1 to Q3, line = median, whiskers = up to 1.5 IQR beyond the box, dots = outliers. On a normal axis the Rs. 35 lakh point would flatten the boxes; the log axis keeps them readable (it is labelled in the plot).

---

## C. Sampling, LLN and CLT

**Q26. What is the Law of Large Numbers?**
As the number of independent observations in a random sample grows, the sample mean gets closer to the true mean of the distribution.

**Q27. How did you demonstrate it?**
We shuffled all rents once (seed 42) and took the first n for n = 10, 25, 50, 100, 250, 500, 1000, 2000, 3000, 4000. For each n we computed the sample mean, compared it with the reference mean (Rs. 34,993.45), and plotted sample size against sample mean with a horizontal line at the reference mean.

**Q28. What do the results show?**
At n = 10 the mean is Rs. 60,850, 74% above the reference mean. By n = 250 it is within about Rs. 1,100, and for n >= 1000 within about Rs. 1,000. Larger samples are generally more stable.

**Q29. The difference at n = 500 is bigger than at n = 250. Does that contradict LLN?** (tricky)
No. LLN is about the long-run tendency, not a guarantee that the error falls at every step. Adding observations can push the mean up or down. We state this in the notebook.

**Q30. Why a random shuffle and not the first n rows?**
The file is sorted by city (Kolkata first, then Mumbai, ...), so the first n rows would all come from one city. A shuffle gives every listing an equal chance.

**Q31. Why nested samples?**
It matches the idea of adding observations to one sample. Each bigger sample contains the smaller ones.

**Q32. Why a fixed seed (random_state=42)?**
For reproducibility: the same table and chart appear every time we run it.

**Q33. Why "reference mean" and not "population mean"?** (tricky)
Our data is a collected sample, not every rental in India, so its mean is not the true population mean. Calling it the reference mean is honest about what we use.

**Q34. Why is 4,746 not one of your sample sizes?**
A sample of the whole dataset would equal the reference mean exactly and show nothing.

**Q35. What is the Central Limit Theorem? How did you show it?**
The means of many random samples are approximately normal with mean mu and standard error sigma/sqrt(n), for large n, whatever the data's shape. We drew 1,000 samples each of n = 30 and n = 100: the spread of the means fell from 12,748 to 7,274 (theory predicts a factor of 1.83; we got 1.75) and the skewness fell from 21 to 2.6 to 1.7.

**Q36. Difference between LLN and CLT?**
LLN: where the sample mean ends up (near the true mean). CLT: how it is distributed around it (roughly normal with SE = sigma/sqrt(n)).

**Q37. Is n >= 30 enough for CLT here?**
Not fully. At n = 30 the sample means are still clearly skewed (2.6) because the rents have heavy tails. Our actual sample is 4,746, which is plenty.

**Q38. What is standard error?**
The standard deviation of the sample mean: s/sqrt(n) = 78,106.41/sqrt(4,746) = Rs. 1,133.76. It falls as n grows.

**Q39. Real-life decision link for LLN (seminar topic)?**
Judging a typical rent from five or ten listings can mislead; with a few hundred the estimate is stable. Insurers, quality control and polling use the same idea.

---

## D. Confidence interval and hypothesis testing

**Q40. What is a confidence interval? What is yours?**
A range of plausible values for the true mean built from the sample. 95% CI for mean rent: Rs. 32,771 to Rs. 37,216.

**Q41. How do you interpret a 95% confidence interval?** (tricky)
If we repeated the sampling many times, about 95% of the intervals built this way would contain the true mean. It is wrong to say "there is a 95% chance the true mean lies in this interval".

**Q42. Why t and not Z for the CI?**
The population SD is unknown, so we use s and the t distribution. With 4,745 degrees of freedom t* = 1.9605, almost the same as Z's 1.96.

**Q43. What makes a CI narrower?**
Larger n, smaller SD, lower confidence level. That is why Bangalore's CI (17,050 to 32,882) is wide and Chennai's (19,440 to 23,788) is narrow.

**Q44. What is a hypothesis test? Steps?**
State H0 and H1, pick alpha (0.05), compute the test statistic and p-value, reject H0 if p < alpha, and interpret.

**Q45. What is a p-value?** (tricky)
The probability of getting a result at least as extreme as ours if H0 were true. It is not the probability that H0 is true, and it does not tell the size of the effect.

**Q46. What are Type I and Type II errors?**
Type I: rejecting a true H0 (probability = alpha = 0.05). Type II: failing to reject a false H0.

**Q47. Which tests did you do and what were H0 and H1?**
(1) Independent two-sample t-test, Furnished vs Unfurnished: H0 the two population mean rents are equal, H1 they are different. (2) One-way ANOVA across the six cities: H0 all six mean rents are equal, H1 at least one city's mean rent is different. Significance level 0.05 for both.

**Q48. Results of the tests?**
t-test: difference of means Rs. 33,649 (furnished higher), pooled SD Rs. 51,678, df = 2,493, t = 14.48, p = 1.07e-45, reject H0. ANOVA: F = 117.64 with 5 and 4,740 degrees of freedom, p = 1.27e-117, reject H0.

**Q49. Explain the independent two-sample t-test in simple words.** (tricky)
We have two separate groups (furnished and unfurnished properties) and ask whether their population mean rents differ. The traditional method assumes both populations have the same variance. Since we do not know the population standard deviation, we combine (pool) the two sample variances into one estimate, s_p^2 = [(n1-1)s1^2 + (n2-1)s2^2]/(n1+n2-2). The t statistic is the difference of the sample means divided by s_p x sqrt(1/n1 + 1/n2). It follows a t distribution with n1 + n2 - 2 degrees of freedom if H0 is true.

**Q50. What are the assumptions of the t-test? Are they satisfied?** (tricky)
Independent observations, roughly normal data and equal population variances. Independence is reasonable. Normality is not true for skewed rent, so we rely on the large groups (680 and 1,815) and the CLT. Equal variances we **assumed but did not verify**; the sample SDs (Rs. 80,631 and Rs. 35,168) differ by about 2.3 times, so the assumption may not hold. We used the traditional test because it is the method taught in the course, and we say openly that the p-value should be read with that caution.

**Q51. If the variances look unequal, why did you still use this test?** (tricky)
For alignment with the course method. We did not claim the assumption is verified; we state it as an assumption and as a limitation. The difference in means is very large compared with its standard error (t = 14.5), so the conclusion is unlikely to change, but we have not proved that.

**Q52. Independent vs paired vs one-sample t-test?**
Independent: two different groups (our case). Paired: the same units measured twice (before/after), tested on the differences. One-sample: one group compared with a stated value. We have no paired data and no benchmark value.

**Q53. Why a t-test and not a Z-test?**
A Z-test needs a known population standard deviation. We do not have one, so we use the sample SD and the t distribution. With this many observations the two would give almost the same answer.

**Q54. How many degrees of freedom does the t-test have and why?**
n1 + n2 - 2 = 680 + 1,815 - 2 = 2,493: one degree of freedom is lost for each of the two sample means used to estimate the variance.

**Q55. How does one-way ANOVA work?**
It compares the variation between the group means (MSB) with the variation within the groups (MSW); F = MSB/MSW, with k - 1 and N - k degrees of freedom (5 and 4,740 for us). A large F means the group means differ more than chance within the groups would explain.

**Q56. Why ANOVA and not several t-tests?**
Six cities give 15 pairs; testing each at 5% raises the chance of at least one false positive. ANOVA tests all groups at once.

**Q57. What does a significant ANOVA tell you?** (tricky)
Only that at least one city's mean rent is different. It does not say that every city differs from every other, and it does not say which ones differ. The city means, confidence intervals and box plot suggest Mumbai is far above the rest and Kolkata is at the other end, but we did not run tests for individual pairs.

**Q58. Why did you not run a post-hoc test?**
It was outside the scope we chose for the project. A post-hoc test such as Tukey's would name the pairs of cities that differ; we list it as future scope.

**Q59. What are ANOVA's assumptions and are they met?** (tricky)
Independent observations, roughly normal data in each group and equal variances. We have not verified normality or equal variances. Rent is strongly skewed and the city SDs range from Rs. 11,137 (Kolkata) to Rs. 120,056 (Bangalore, inflated by one Rs. 35,00,000 listing), so the equal-variance assumption looks doubtful. The groups are large (524 to 972), which helps, but the F value and p-value should be read with caution.

**Q60. With n = 4,746 almost anything is significant. Does significant mean important?** (tricky)
No. Significance only says the difference is unlikely to be chance. We therefore also report the size of the difference: furnished flats are about Rs. 33,649 higher on average.

**Q61. Does the furnished vs unfurnished test prove furnishing raises rent?**
No. Furnished flats may also be larger or more often in Mumbai (confounding). It is a raw comparison; the medians (27,000 vs 12,000) show the gap is not just due to extreme values.

**Q62. Does "fail to reject H0" mean H0 is true?**
No, only that we do not have enough evidence against it. (It did not come up in our results, since both tests rejected H0.)

---

## E. Covariance and correlation

**Q63. What is covariance?**
A measure of whether two numerical variables tend to rise together (positive) or move oppositely (negative). Its size depends on the units.

**Q64. What is correlation?**
The strength and direction of the linear relationship between two numerical variables, between -1 and +1.

**Q65. Covariance vs correlation?**
Correlation = covariance / (s_X x s_Y). It is unit-free and comparable across pairs; covariance is not.

**Q66. Why Rent, Size, BHK and Bathroom?**
Covariance and correlation need numerical variables. City and Furnishing Status are categorical, so they cannot go into the formulas (they are handled by t-tests, ANOVA and dummy variables).

**Q67. Results?**
Pearson: Rent-Size 0.41, Rent-BHK 0.37, Rent-Bathroom 0.44 (moderate); Size-BHK 0.72, Size-Bathroom 0.74, BHK-Bathroom 0.79 (strong). Rent-Size covariance is 20,485,348, large only because the units are rupees and square feet.

**Q68. What is Spearman correlation and why did you add it?**
Pearson on the ranks. It measures consistent rising (monotonic) and is not distorted by outliers. Rent is very skewed, so we report both.

**Q69. Why is Spearman higher than Pearson for Rent?**
Rent rises consistently with Size, BHK and Bathroom (Spearman 0.52, 0.57, 0.66), but a few extreme rents pull the Pearson values down (0.41, 0.37, 0.44).

**Q70. Does correlation mean causation?**
No. Larger flats cost more, but size is also tied to city, building quality and other factors.

**Q71. Size, BHK and Bathroom correlate 0.72 to 0.79. Is that a problem?**
It is multicollinearity: predictions stay fine but the individual regression coefficients become less stable.

**Q72. Can a correlation near zero mean there is no relationship?**
No, only no linear (or monotonic) relationship; a curved relationship could still exist.

---

## F. Regression

**Q73. What is regression?**
Fitting an equation that describes and predicts a response (Rent) from one or more predictors.

**Q74. What is your simple regression model?**
Rent = -12,678.18 + 49.46 x Size. Each extra sq. ft. adds about Rs. 49 to the predicted rent on average.

**Q75. How are the slope and intercept found?**
Least squares: b1 = sum(x - x-bar)(y - y-bar) / sum(x - x-bar)^2 and b0 = y-bar - b1 x-bar, which minimise the sum of squared errors. We check against scipy's linregress (same values).

**Q76. What does the negative intercept mean?**
Nothing practical: it is the predicted rent at size 0, outside the data. It also shows the straight line is not ideal, since very small flats get negative predicted rents.

**Q77. What are MAE, MSE, RMSE and R-squared?**
MAE: average absolute error. MSE: average squared error. RMSE: its square root, in rupees. R-squared: share of the variation in Rent explained (1 = perfect, 0 = no better than the mean).

**Q78. What are your simple-model results?**
Train R-squared 0.15, test R-squared 0.32, test MAE Rs. 27,807, test RMSE Rs. 52,189. Size alone is a weak predictor.

**Q79. Why is the test R-squared higher than the training R-squared?** (tricky)
The Rs. 35 lakh Bangalore listing fell into the training rows and inflates the training errors. Without it the training R-squared is 0.25. It shows a single extreme row can move R-squared a lot.

**Q80. Why did you split the data into train and test?**
To check how the model does on listings it has not seen; good training scores alone can hide overfitting. 80% (3,797 rows) to fit, 20% (949 rows) to test, seed 42.

**Q81. What are the assumptions of linear regression? Are they met?**
Linearity, independent errors, constant variance, normal errors (and no perfect multicollinearity). For raw Rent the residual plot fans out and is skewed, so constant variance and normality fail; the log model's residuals are evenly spread.

**Q82. Why did you build a multiple regression and what are its predictors?**
Size alone explains little; rent also depends on rooms and city. Predictors: Size, BHK, Bathroom and City (as 0/1 dummies, Kolkata as the baseline).

**Q83. What are dummy variables? Why drop one city?** (tricky)
A categorical variable is turned into 0/1 columns, one per category. One category is dropped as the baseline; keeping all would make the columns add up to the intercept column (perfect multicollinearity, the "dummy variable trap"). Each city coefficient is the difference from Kolkata.

**Q84. How do you read the log model coefficients?**
ln(Rent) = a + b x means a one-unit rise in x multiplies rent by e^b. Mumbai b = 1.537, e^1.537 = 4.65, so rent is about 365% higher than in Kolkata for a comparable flat; each extra BHK about +26%, bathroom +20%, each 100 sq. ft. about +5%.

**Q85. Which of your three models is the best and why?**
The multiple model on log(Rent): on the test rows MAE about Rs. 12,230 against Rs. 22,345 (multiple on Rent) and Rs. 27,807 (simple); log-scale R-squared 0.77 train and 0.79 test (adjusted 0.773), so it explains about three quarters of the variation with no sign of overfitting.

**Q86. The rupee R-squared for the log model on training rows is only 0.28. Is the model bad?** (tricky)
No. That number is on the rupee scale, where the Rs. 35 lakh listing dominates the total variation in the training rows. On the log scale, where the model was fitted, R-squared is 0.77. Compare models on test MAE/RMSE, and always say which scale an R-squared is on.

**Q87. What is adjusted R-squared?**
R-squared penalised for the number of predictors, so adding a useless variable does not raise it. For the log model it is 0.7734 (R-squared 0.7738), so every predictor earns its place.

**Q88. What is multicollinearity and does it affect predictions?**
Predictors that are strongly correlated with each other (Size, BHK, Bathroom, 0.72 to 0.79). Predictions are fine but individual coefficients are unstable. A VIF check would quantify it (not done).

**Q89. Why does the raw multiple model say Chennai and Hyderabad are cheaper than Kolkata?**
That is an artefact of fitting skewed rent with a straight-line model; after controlling for size and bathrooms the signs flip. In the log model all cities are above Kolkata, in the order of their averages.

**Q90. Can you predict rent with it? Any caution?**
Yes, within the range of the data. Cautions: the back-transformed log prediction is a typical (median-like) rent, not a mean; do not extrapolate (for example above 8,000 sq. ft.); results come from one split; it shows association, not causation.

**Q91. Why linear regression and not something fancier?**
It is what the syllabus covers and every coefficient can be explained in words.

---

## G. Conclusion and judgement

**Q92. What are your main findings?**
Rent is right-skewed (mean 34,993 vs median 16,000); Mumbai is by far the dearest and Kolkata the cheapest; rent differs significantly by city and by furnishing; rent is moderately correlated with size, BHK and bathrooms; a multiple regression on log(Rent) explains about three quarters of the log-rent variation on unseen listings.

**Q93. What are the limitations of your study?**
Convenience sample from six cities and three months of 2022; a few unusual listings kept; skewed data so the CI, t-test and ANOVA rely on large n; the t-test and ANOVA assume equal variances, which we did not verify (the group SDs differ a lot); no tests for individual pairs of cities; raw group comparisons without controls; one train/test split; association not causation.

**Q94. What would you do next?**
Add more variables (floor, locality), use cross-validation, check the extreme listings against the source, run a post-hoc Tukey test, and collect newer data from more cities.

**Q95. If we gave you a new city's data, what would you do differently?**
Check the sampling method first, repeat the cleaning checks and plots, and only then test and model. In particular, check whether the new data is a random sample of the market.

**Q96. What did each team member do?** *(fill in before the viva; see report/NOTES.md)*

---

## H. Quick one-liners (course topics, in case sir drifts)

- **Binomial:** n independent yes/no trials with success probability p; P(X=k) = C(n,k) p^k (1-p)^(n-k).
- **Poisson:** counts of events in an interval at rate lambda; P(X=k) = e^-lambda lambda^k / k!.
- **Normal / Z:** bell curve; z = (x - mu)/sigma standardises any normal variable.
- **Conditional probability / Bayes:** P(A|B) = P(A and B)/P(B); Bayes updates a probability using new evidence. (Not used in this project; rent is continuous and skewed, not a count or yes/no outcome.)
