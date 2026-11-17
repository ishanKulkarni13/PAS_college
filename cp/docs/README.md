# Property Location and Rental Price Analysis

**Course Project Topic:** Property Location and Rental Price Analysis  
**Group Seminar Topic:** Law of Large Numbers in Real-Life Decision Making  

---

## Dataset

**Name:** House Rent Prediction Dataset  
**Source:** https://www.kaggle.com/datasets/iamsouravbanerjee/house-rent-prediction-dataset  
**Author:** iamsouravbanerjee  

The dataset contains 4,746 rental property listings across 6 Indian cities, with 12 columns. Each row represents one listing.

---

## Objective

Study how rental prices vary across property locations and characteristics, and demonstrate the Law of Large Numbers using random samples of the rental price data.

---

## Statistical Measures Used

- Mean
- Median
- Mode
- Variance
- Standard Deviation
- IQR (Interquartile Range)
- Range
- Covariance
- Correlation

---

## LLN Demonstration

Random samples of increasing size (10, 25, 50, 100, 250, 500, 1000, 2000, 3000, 4000, 4746) are drawn from the dataset. The sample mean is calculated for each size and compared with the reference mean (mean of the complete dataset). As sample size grows, the sample mean converges toward the reference mean.

---

## Main Variables Used

- **Rent** - main variable, monthly rental price
- **City** - main location variable
- **Size** - property area in sq. ft.
- **BHK** - number of bedrooms, hall, and kitchen
- **Bathroom** - number of bathrooms

---

## How to Run

1. Make sure Python 3 is installed with pandas, numpy, and matplotlib.
2. Place the dataset at `data/House_Rent_Dataset.csv` (already done).
3. Open `property_location_rental_price_analysis.ipynb` in Jupyter Notebook or JupyterLab.
4. Run all cells from top to bottom (Kernel - Restart and Run All).
