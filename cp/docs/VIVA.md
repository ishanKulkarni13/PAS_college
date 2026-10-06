# VIVA Questions and Answers

---

**Q1. What is the objective of your project?**

We are studying how rental prices vary across different cities and property characteristics. We also demonstrate the Law of Large Numbers using random samples of the rental price data.

---

**Q2. Why did you choose this dataset?**

The House Rent Prediction dataset from Kaggle has a good set of variables for this project. It has Rent as the main numerical variable, City as the location variable, and Size as a numerical variable for covariance and correlation. It is a real dataset with no fabricated values.

---

**Q3. What is the main variable in your analysis?**

Rent. It is the monthly rental price in rupees. All the required statistical measures are calculated for Rent.

---

**Q4. What is the mean?**

The mean is the sum of all values divided by the number of values. For this dataset, the mean rent is Rs. 34,993.

---

**Q5. What is the median?**

The median is the middle value when the data is arranged in order. For an even number of observations, it is the average of the two middle values. The median rent in our dataset is Rs. 16,000.

---

**Q6. Why can the mean and median be different?**

If the data is skewed or has extreme values, the mean gets pulled in the direction of the extreme values. In our dataset, there are some very high-rent properties, especially in Mumbai, which pull the mean upward. The median is not affected by these extreme values, so it stays closer to the typical rent.

---

**Q7. What is the mode?**

The mode is the most frequently occurring value in the data. The mode of Rent in our dataset is Rs. 15,000.

---

**Q8. What is variance?**

Variance measures how spread out the values are around the mean. We use sample variance (dividing by n-1). The variance of Rent is about 6.1 billion square rupees, which is a very large number because the unit is rupees squared.

---

**Q9. What is standard deviation?**

Standard deviation is the square root of variance. It is in the same unit as the original variable, so it is easier to interpret. The standard deviation of Rent is Rs. 78,106. This means rental prices are very spread out around the mean.

---

**Q10. What is IQR?**

IQR stands for Interquartile Range. It is Q3 minus Q1, where Q1 is the 25th percentile and Q3 is the 75th percentile. IQR gives the spread of the middle 50% of the data. For our dataset, IQR = Rs. 33,000 - Rs. 10,000 = Rs. 23,000.

---

**Q11. What is range?**

Range is the difference between the maximum and minimum values. The minimum rent is Rs. 1,200 and the maximum is Rs. 35,00,000. The range is Rs. 34,98,800.

---

**Q12. What is covariance?**

Covariance tells us whether two numerical variables tend to increase together or move in opposite directions. A positive covariance means both variables tend to increase together. A negative covariance means one increases when the other decreases.

---

**Q13. What is correlation?**

Correlation tells us the strength and direction of the linear relationship between two numerical variables. It is a value between -1 and +1. Values close to +1 mean a strong positive relationship, values close to -1 mean a strong negative relationship, and values close to 0 mean little or no linear relationship.

---

**Q14. What is the difference between covariance and correlation?**

Covariance gives the direction of the relationship between two variables, but its value depends on the units of both variables, so it is hard to interpret. Correlation standardizes covariance by dividing by the product of both standard deviations, giving a value between -1 and +1 that is easy to interpret regardless of units.

---

**Q15. Why did you use Size and Rent for covariance?**

Covariance requires both variables to be numerical. City is a categorical variable, so it cannot be used directly. Size and Rent are both numerical, so they are the right choice. Also, it makes practical sense to ask whether larger properties tend to cost more.

---

**Q16. What does the correlation of 0.41 between Size and Rent tell you?**

It is a moderate positive correlation. Larger properties do tend to have higher rents in this dataset, but the relationship is not very strong. Many other factors like city, furnishing, and floor also affect rent.

---

**Q17. Does correlation mean causation?**

No. A correlation between Size and Rent means they tend to increase together in this data, but it does not mean that increasing the size of a property causes the rent to increase. There could be other factors involved.

---

**Q18. What is the Law of Large Numbers?**

The Law of Large Numbers says that as the number of observations in a random sample increases, the sample mean tends to get closer to the true mean of the distribution.

---

**Q19. How did you demonstrate the Law of Large Numbers?**

We took random samples of different sizes from the Rent column: 10, 25, 50, 100, 250, 500, 1000, 2000, 3000, 4000, and 4746 observations. For each sample, we calculated the sample mean and compared it with the reference mean of the complete dataset. We also plotted sample size on the x-axis and sample mean on the y-axis, with a horizontal line for the reference mean.

---

**Q20. Why did you use random sampling instead of taking the first n rows?**

Taking the first n rows would not give a representative sample because the data might be ordered in some way. Random sampling gives every observation an equal chance of being selected, which is what LLN requires.

---

**Q21. Why did you use a fixed random seed?**

A fixed random seed makes the sampling reproducible. Every time the notebook is run, the same rows are selected, so the results are consistent. This is a simple but useful programming practice.

---

**Q22. Why did you use the complete dataset mean as a reference instead of calling it the population mean?**

The Kaggle dataset is a collection of listings from one source, not a census of all rental properties in India. So it is not correct to call its mean the population mean. We call it the reference mean to be accurate about what we are actually using.

---

**Q23. Why did you not use machine learning?**

Our project is a statistics course project, not a machine learning project. The requirement from our professor is to perform statistical analysis using measures like Mean, Median, Mode, Variance, Standard Deviation, IQR, Range, Covariance, and Correlation. There is no need for prediction models here.

---

**Q24. What are the limitations of your analysis?**

The main limitations are: the dataset is from one source and one time period, so it may not represent the current rental market. A few listings have unusually small sizes that could be errors. The correlation between Size and Rent does not imply causation. And the reference mean used for LLN is the dataset mean, not the true population mean.
