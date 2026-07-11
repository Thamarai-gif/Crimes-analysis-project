# Crime Data Analysis - Part 1

## Overview
This notebook focuses on cleaning the dataset and preparing it for modeling.

## Data Cleaning
- Converted categorical columns (`Primary Type`, `Location Description`) to category dtype.
- Imputed missing values using median for skewed numeric columns.
- Verified no nulls remain.
- Saved final dataset as `cleaned_data.csv`.

## Correlation Analysis
- Pearson correlation (Year vs Arrest): -0.036
- Spearman correlation: -0.028
- Difference: 0.009 → weak, approximately linear.
- Spearman chosen for feature selection guidance.

## Group Statistics
- Highest mean arrest rate: **GAMBLING (1.00)**
- Highest std: **Public Peace Violation (~0.50)**
- Ratio of highest to lowest mean: ~62.5 → strong predictive signal.

## Output
- Cleaned dataset saved as `cleaned_data.csv`



Crime Data Analysis – Part A (Preprocessing & Visualization)
1. Dataset Overview :
•	Columns used: Primary Type, Location Description, Arrest, Domestic, Year
•	Dropped: ID (unique identifier, not useful for analysis)


3. Preprocessing Steps :
•	Converted repetitive string columns (Primary Type, Location Description) to category dtype for memory optimization
•	Reported memory usage before and after conversion using df.memory_usage(deep=True).sum()
•	Checked skewness for numeric columns (Year, Arrest, Domestic) and identified the column with highest absolute skewness
•	Computed IQR, lower/upper bounds, and counted outliers (8837 rows detected)



5. Outlier Handling (Part A Decision) :
•	Outliers detected but not capped or removed in Part A
•	Decision: Handle them in Part B (feature engineering) using capping or transformations


7. Visualization :
•	Boxplots for numeric columns to show outliers
•	Histograms for distribution of Year, Arrest, Domestic
•	Bar charts for categorical columns (Primary Type, Location Description)


The histogram below shows the distribution of crime records across the Year column.
•	Shape of the distribution: The distribution is highly skewed, with a sharp peak in 2023 where the majority of records are concentrated. Other years (2021, 2022, 2024, 2025) have relatively few records, making the dataset imbalanced. This skewness indicates that most of the data comes from a single year, which may affect model training and predictions.

Scatter Plot: Year vs Arrest
The scatter plot below shows the relationship between the numeric columns Year and Arrest (0 = No, 1 = Yes).
•	Shape of the distribution: The points are concentrated around the year 2023, reflecting the dataset imbalance we saw earlier.
•	Direction of relationship: There is no strong linear correlation between Year and Arrest — arrests occur in both 0 and 1 categories across years.
•	Approximate strength: The relationship is weak, since Arrest is binary and does not vary continuously with Year. The clustering in 2023 is due to the dataset skew, not a true correlation.

Correlation Analysis
The correlation matrix of numeric columns (Year and Arrest) shows a very weak negative correlation (-0.04).
•	Causal relationship? This weak correlation does not imply causation. The fact that arrests appear slightly less frequent in certain years does not mean that the year itself causes changes in arrest rates.
•	Third variable explanation: A third factor is likely driving the pattern. For example, reporting practices or data collection bias could explain why most records cluster in 2023. The imbalance in the dataset makes the correlation appear, but it is not a meaningful causal link.
•	Plausible alternative explanation: One plausible explanation is that changes in law enforcement policy or data availability in 2023 led to a surge in recorded cases. This would explain the skew without suggesting that the year itself directly influences arrests.

R
Spearman vs Pearson Correlation We compared the Pearson and Spearman correlation matrices for all numeric columns. The three column pairs with the largest absolute differences between Spearman and Pearson correlations were identified.
For each pair:
1.	If |Spearman| > |Pearson| → The relationship is monotonic but non-linear. This means the variables move together consistently, but not proportionally.
2.	If |Pearson| ≥ |Spearman| → The relationship is approximately linear, so Pearson captures it well.
Choice for Feature Selection: We will rely primarily on Spearman correlation for feature-selection guidance in Part 2.
•	Reason: Spearman is more robust to skewness and non-linear monotonic relationships, which are common in categorical-heavy datasets like ours.
•	Pearson assumes linearity, which may miss important monotonic trends.




5. Key Insights :
•	Memory reduced significantly after converting string columns to category
•	Outliers exist in Year column (8837 rows)
•	Skewness analysis shows imbalance in numeric columns
•	Dataset is now clean and ready for visualization

