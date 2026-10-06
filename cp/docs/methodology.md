# Methodology

## Dataset Selection

We used the House Rent Prediction Dataset from Kaggle (by iamsouravbanerjee) because it contains rental property listings from Indian cities and includes the key variables needed for the project: Rent (numerical, main variable), City (categorical, location variable), and Size (numerical, suitable for covariance and correlation).

---

## Data Cleaning

After loading the dataset, we checked for:

- **Missing values:** None found. All 4,746 rows are complete.
- **Duplicate rows:** None found.
- **Invalid values:** Rent and Size are both positive integers. A small number of listings have very low Size values (below 50 sq. ft.), which we retained in the dataset since they cannot be confirmed as errors.

No rows were removed from the dataset. The analysis uses all 4,746 listings.

---

## Descriptive Statistics

The following measures were calculated for Rent (and summarized for Size, BHK, and Bathroom):

- **Mean:** Average of all values.
- **Median:** Middle value in the sorted list. More robust to extreme values than the mean.
- **Mode:** Most frequently occurring value. For Rent, it is Rs. 15,000.
- **Variance:** Average squared deviation from the mean. We use sample variance (ddof=1).
- **Standard Deviation:** Square root of variance. In the same unit as Rent, so it is easier to interpret.
- **IQR:** Q3 - Q1. Gives the spread of the middle 50% of the data, not affected by extreme values.
- **Range:** Maximum minus minimum. Shows the full spread.

Rent is right-skewed. The mean (Rs. 34,993) is much higher than the median (Rs. 16,000) because of a small number of very high-rent listings, especially in Mumbai. This is why both the mean and median are reported.

---

## Location Analysis

City is used as the main location variable. For each of the 6 cities (Mumbai, Chennai, Bangalore, Hyderabad, Delhi, Kolkata), we calculate the count, mean, median, and standard deviation of Rent. A bar chart shows the average rent by city.

Area Locality is not used as a main analysis variable because the dataset has too many unique locality names. City gives a cleaner and more interpretable comparison.

---

## Covariance

We calculate the sample covariance between Size and Rent using `numpy.cov(Size, Rent, ddof=1)`. Both variables are numerical, which is required for covariance to make sense. The result is positive, meaning larger properties tend to have higher rents.

Covariance is not calculated between a categorical variable (like City) and Rent, because the formula requires numerical inputs.

---

## Correlation

We calculate the Pearson correlation coefficient between Size and Rent using `df["Size"].corr(df["Rent"])`. The result is 0.4136, which is a moderate positive correlation.

The difference between covariance and correlation is important:
- Covariance gives the direction of the relationship but depends on the scale of both variables.
- Correlation standardizes this into a value between -1 and +1, making it comparable across variable pairs.

---

## Law of Large Numbers

**Reference mean:** We use the mean of the complete available dataset as the reference mean (Rs. 34,993.45). We do not call this the population mean of all Indian rental properties, because the dataset is a collected sample, not a census.

**Random sampling:** For each sample size, we use `df["Rent"].sample(n=n, random_state=42)`. Random sampling ensures every observation has an equal chance of being included, unlike simply taking the first n rows.

**Fixed seed:** `random_state=42` is used so the results are reproducible. Running the notebook again gives the same sample selections.

**Sample sizes:** 10, 25, 50, 100, 250, 500, 1000, 2000, 3000, 4000, 4746. The maximum is 4746, which is the full dataset (giving the reference mean exactly).

**Observation:** At small sample sizes, the sample mean can differ significantly from the reference mean. As sample size increases, the sample mean fluctuates less and settles closer to the reference mean. This is the Law of Large Numbers applied to real rental price data.

---

## Limitations

- The dataset is from one Kaggle source and represents listings from a specific time period.
- Some properties in the dataset may have unusual characteristics (very small size, very high rent) that are kept in the analysis.
- Correlation does not imply causation. A positive correlation between Size and Rent means they tend to increase together in this data, not that size causes rent.
- The reference mean used in the LLN demonstration is the mean of the available dataset, not the true mean of all Indian rental properties.
