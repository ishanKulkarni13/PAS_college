# 12. Limitations, Future Scope, Applications and References

Notebook cell 92. Report sections 7.3, 7.4, 8.2 and References.

## Limitations of the study (report 7.4)

**Data**
- One non-random collection of listings from six cities in about three months of 2022 (convenience sample). Results describe this dataset, not necessarily the whole market or today's prices.
- Only listed properties; asking rent, not final agreed rent.
- A few unusual rows (16 sizes below 50 sq. ft.; one Rs. 35,00,000 rent in Bangalore, probably a data-entry error but unproven). They were kept and affect means, SDs and the raw regression.

**Methods**
- Rent is far from normal, so the CI, the t-test and the ANOVA lean on the large sample (CLT).
- The traditional (pooled) t-test and the one-way ANOVA both assume equal variances. We assumed this and did **not verify** it. The group SDs differ a lot (furnished Rs. 80,631 vs unfurnished Rs. 35,168; cities from Rs. 11,137 in Kolkata to Rs. 120,056 in Bangalore), so the t statistic, F value and p-values should be read with caution.
- A significant ANOVA only shows that at least one city's mean differs; we did not run tests for individual pairs of cities (no post-hoc test).
- The Furnished vs Unfurnished comparison is a raw group comparison; it does not control for size, city or other factors (confounding).
- Regression uses only a few variables and a single 80/20 split; no cross-validation. Back-transformed log predictions are median-like, not mean.
- Size, BHK and Bathroom are strongly correlated (multicollinearity), so individual coefficients are less stable.
- All results are associations; none proves causation.

## Future scope (report 8.2)

- Add more variables: floor, area locality, tenant preference, point of contact, and the posting date.
- Use cross-validation instead of a single split; try other models (for example regularised regression) for comparison.
- Check the extreme listings against the original source, and repeat the analysis with and without them.
- Check the equal-variance assumption formally and, if it fails, use a test that does not need it; add a post-hoc test (Tukey) after ANOVA to name which city pairs differ.
- Test for interaction (does the effect of size differ by city?) and use a VIF check for multicollinearity.
- Collect newer, larger data from more cities and a random sampling design.

## Practical applications and implications (report 7.3)

Text material for this section (no code needed):

- **Tenants and house-hunters:** know a realistic rent range for a city/size, and see that asking prices for the same flat vary widely, so compare several listings (LLN: a few listings can mislead, many give a stable picture).
- **Landlords and agents:** price a property using size, BHK, bathrooms and city; the log model gives a data-based starting point (typical error about Rs. 12,000 on listings it has not seen).
- **Real-estate platforms and analysts:** report the median, not only the mean, because rent is skewed; flag extreme listings for review.
- **City planners and policy makers:** large between-city gaps (Mumbai vs Kolkata) show where affordability pressure is highest.
- **Statistical lesson:** skewed data needs medians, rank-based methods or a log transform; one extreme value can change a regression; significance (p-value) is not the same as importance (size of the difference).

## References (candidates, IEEE style, from the syllabus and the tools used)

1. R. E. Walpole, R. H. Myers, S. L. Myers, and K. Ye, *Probability and Statistics for Engineers and Scientists*, 9th ed. Upper Saddle River, NJ, USA: Pearson/Prentice Hall, 2012.
2. D. M. Diez and M. Cetinkaya-Rundel, *OpenIntro Statistics*, 3rd ed. OpenIntro Inc., 2015.
3. G. James, D. Witten, T. Hastie, and R. Tibshirani, *An Introduction to Statistical Learning with Applications in R*. New York, NY, USA: Springer, 2013.
4. B. Illowsky and S. Dean, *Introductory Statistics*. Houston, TX, USA: OpenStax, 2018.
5. S. Banerjee, "House Rent Prediction Dataset," Kaggle. https://www.kaggle.com/datasets/iamsouravbanerjee/house-rent-prediction-dataset
6. W. McKinney, "Data structures for statistical computing in Python," in *Proc. 9th Python in Science Conf.*, 2010, pp. 56-61.
7. P. Virtanen et al., "SciPy 1.0: Fundamental algorithms for scientific computing in Python," *Nature Methods*, vol. 17, pp. 261-272, 2020.
8. J. D. Hunter, "Matplotlib: A 2D graphics environment," *Computing in Science & Engineering*, vol. 9, no. 3, pp. 90-95, 2007.

Items 1 to 4 are the syllabus textbooks; 6 to 8 are the standard citations for pandas, SciPy and Matplotlib. The team should confirm the dataset author's name format and the access date before final submission. The in-text citation numbers [1], [2]... in the report must match this list.
