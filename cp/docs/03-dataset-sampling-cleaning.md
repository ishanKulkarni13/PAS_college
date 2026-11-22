# 03. Dataset, Sampling and Cleaning

Notebook cells 2-16 and 79. Report sections 2.1 to 2.4.

## Dataset features and variables (report 2.1)

| Column | Type | Used? | Notes |
|---|---|---|---|
| Posted On | Date | No (used only for the date window) | 13 Apr 2022 to 11 Jul 2022 |
| BHK | Numerical, discrete | Yes | 1 to 6; mean 2.08, mode 2 |
| **Rent** | Numerical, continuous | **Yes, main variable** | Rs. 1,200 to Rs. 35,00,000 |
| Size | Numerical, continuous | Yes | 10 to 8,000 sq. ft.; mean 967.49 |
| Floor | Categorical (text like "1 out of 3") | No | Needs parsing; not needed |
| Area Type | Categorical | No | Super Area 2,446; Carpet Area 2,298; Built Area 2 |
| Area Locality | Categorical | No | 2,235 unique values: too many for a clean comparison |
| **City** | Categorical | **Yes, main location variable** | 6 cities |
| Furnishing Status | Categorical | Yes (t-test) | Semi-Furnished 2,251; Unfurnished 1,815; Furnished 680 |
| Tenant Preferred | Categorical | No | Bachelors/Family 3,444; Bachelors 830; Family 472 |
| Bathroom | Numerical, discrete | Yes | 1 to 10; mean 1.97, mode 2 |
| Point of Contact | Categorical | No | Owner 3,216; Agent 1,529; Builder 1 |

**Types of data (syllabus topic):** numerical (can do arithmetic: Rent, Size, BHK, Bathroom) and categorical (labels: City, Furnishing Status). Discrete means countable values (BHK, Bathroom); continuous means any value in a range (Rent, Size). Covariance, correlation and regression need numerical variables; categorical variables can be compared by groups (t-test, ANOVA) or turned into 0/1 dummy columns for regression.

**Why City and not Area Locality as the location variable:** the locality column has 2,235 distinct names for 4,746 rows (about 2 listings per locality), so groups would be tiny. City gives six sizeable groups that are easy to interpret.

## Sampling method and sample size (report 2.2)

- The dataset is a Kaggle collection of rental listings. The author does not describe a random sampling plan.
- So we treat the 4,746 listings as **one non-probability (convenience) sample** of the Indian rental market. It is not a simple random sample and not a census.
- **Sample size:** n = 4,746. Per city: Mumbai 972, Chennai 891, Bangalore 886, Hyderabad 868, Delhi 605, Kolkata 524. The sample is large, which is why means, CIs and tests behave well even though rent is skewed (see 05 and 06).
- **Random sampling inside the project:** the LLN and CLT demonstrations draw random samples *from* this dataset with fixed seeds (see 05). That is a different thing from how the dataset itself was collected.

### Types of sampling techniques (for the viva)

| Technique | Idea | Is it ours? |
|---|---|---|
| Simple random sampling | Every unit has an equal chance | Used only inside the LLN/CLT demos |
| Stratified | Random sample from each group (e.g. each city) | No |
| Cluster, systematic | Sample whole clusters / every k-th unit | No |
| Convenience (non-probability) | Whatever data is available | **Yes, the dataset itself** |

## Sampling bias and limitations (report 2.4)

- Only listed properties appear: informal or very cheap rentals may be missing.
- Only six big cities; no smaller cities or rural areas.
- One short period (about three months of 2022): prices today may differ.
- Owners/agents choose what to post and what to charge (listing price, not final agreed rent).
- Cities are not equally represented (Mumbai 972 vs Kolkata 524), which affects pooled numbers such as the overall mean.
- Consequence: all results describe this dataset; extending them to "all rentals in India" needs care. That is why we say "reference mean", not "population mean".

## Data cleaning and preprocessing (report 2.3)

| Check | Result | Cell |
|---|---|---|
| Missing values | None in any of the 12 columns | 6 |
| Duplicate rows | 0 | 7 |
| Rent <= 0 or Size <= 0 | 0 and 0 | 15 |
| Very small sizes (< 50 sq. ft.) | 16 rows (e.g. 3 BHK with 10 or 25 sq. ft.); **kept** | 16 |
| Extreme rents (IQR rule: outside Rs. -24,500 to Rs. 67,500) | 520 listings (about 11%); **kept** | 32 |
| Largest rent | Rs. 35,00,000, a 3 BHK of 2,500 sq. ft. in Bangalore (the city's median is Rs. 14,000) | 84 |

**Decision: nothing was removed.** The 16 tiny sizes and the extreme rents cannot be proved to be errors; deleting them would be a choice that changes the results. Instead we report their effect (for example, how the one Rs. 35 lakh row changes the regression, cell 84) and use methods that cope with them (median and Spearman correlation). If sir asks: "we checked, we flagged, we did not silently delete."

**IQR outlier rule (cell 32):** a value is an outlier if it is below Q1 - 1.5 x IQR or above Q3 + 1.5 x IQR. With Q1 = 10,000, Q3 = 33,000, IQR = 23,000 the bounds are -24,500 and 67,500. The lower bound is negative so no low outliers exist; 520 listings are above 67,500.

**Other preprocessing:**
- City converted to 0/1 dummy columns for the multiple regression, Kolkata as baseline (cell 87).
- 80/20 random train/test split with seed 42: 3,797 training rows and 949 test rows (cell 79).
