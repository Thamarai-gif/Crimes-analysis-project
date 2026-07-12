Label Definitions (Part 2)
	Regression Label (y_reg) A continuous numeric column representing the target variable for regression tasks (e.g., income, price, salary).
	Used to predict actual numeric values.
	Example: y_reg = df["Arrest"]
Classification Workflow (y_clf)
	Target: Binary column (either derived from y_reg at its median or a natural binary column like domestic).

Why One Hot Encoding?
Categorical variables like city names or color codes have no natural order. If we applied label encoding, the categories would be converted into integers (e.g., Red = 1, Blue = 2, Green = 3).
This creates a false ordinal relationship:
	The model might incorrectly assume that Green (3) is “greater” than Blue (2) or that the “distance” between Red (1) and Blue (2) is meaningful.
	In reality, these categories are just different labels, not ordered values.
One Hot Encoding avoids this problem by creating separate binary columns for each category:
	Red → [1,0,0]
	Blue → [0,1,0]
	Green → [0,0,1]
These are the examples to understand one hot encoding.


Feature Scaling and Data Leakage
We apply StandardScaler to normalize feature values before training models.
	Correct procedure:
	Fit the scaler only on the training features (scaler.fit(X_train)).
	Transform both training and test sets using this fitted scaler (scaler.transform(...)).
	Why not fit on the full dataset? Fitting the scaler on the entire dataset would constitute data leakage.
	The scaler would compute statistics (mean and standard deviation) using both training and test data.
	This encodes information from the test set into the training process, giving the model unfair knowledge of unseen data.
	As a result, evaluation metrics would be artificially inflated and not reflect true generalization performance.


Interpreting Regression Coefficients
	Large Positive Coefficient A one unit increase in the scaled feature is associated with an increase of that coefficient’s value in the predicted target.
	Example: If city_Bangalore has a coefficient of +2.5, then moving one unit higher in that scaled feature corresponds to a +2.5 unit increase in the predicted outcome (e.g., income).
	In plain terms: the feature has a strong positive influence on the prediction.
	Large Negative Coefficient A one unit increase in the scaled feature is associated with a decrease of that coefficient’s value in the predicted target.
	Example: If color_Red has a coefficient of -3.0, then moving one unit higher in that scaled feature corresponds to a −3.0 unit decrease in the predicted outcome.
	In plain terms: the feature has a strong negative influence on the prediction.


Model Comparison: Linear vs Ridge Regression
Model	MSE (Test)	R² (Test)
Linear Regression	[your MSE]	[your R²]
Ridge Regression	[your MSE]	[your R²]


Why Ridge Regression Differs
Ridge Regression introduces an L2 penalty that shrinks coefficient values toward zero. This regularization reduces the impact of multicollinearity (highly correlated features, common after one hot encoding) and prevents overfitting. As a result, Ridge often produces a different coefficient profile than plain OLS Linear Regression: coefficients are smaller and more evenly distributed, rather than extreme.
The alpha parameter controls the strength of this penalty:
	Low alpha (≈0) → behaves like standard Linear Regression.
	Higher alpha → stronger shrinkage, more bias but less variance, often improving generalization.


Classification Metrics
Formulas:
	Precision
"Precision"=TP/(TP+FP)
Where:
	TP= True Positives
	FP= False Positives
	Recall
"Recall"=TP/(TP+FN)
Where:
	TP= True Positives
	FN= False Negatives
Which metric matters more here? For this classification task, Recall is more important if false negatives are more costly than false positives. For example, if the model is predicting whether a student is at risk of not being placed, missing a true positive (a student who actually won’t get placed) could mean failing to provide necessary support. In that case, Recall ensures we catch as many true cases as possible, even if it means tolerating some false alarms.
AUC Interpretation: The Area Under the ROC Curve (AUC) measures the model’s ability to separate the two classes across all possible thresholds.
	An AUC of 1.0 means the model perfectly distinguishes between the classes (every positive ranked above every negative).
	An AUC of 0.5 would mean the model is no better than random guessing.
	Higher AUC values indicate stronger discriminative power.
In our case, the AUC = 1.00, which indicates the model achieved perfect separation between the classes on the test set.



Classification Metrics and Threshold Tuning
Formulas:
	Precision
"Precision"=TP/(TP+FP)
	Recall
"Recall"=TP/(TP+FN)
Where:
	TP= True Positives
	FP= False Positives
	FN= False Negatives
Threshold that maximises F1-score: From the evaluation table (thresholds 0.30 → 0.70), the threshold with the highest F1-score was [insert your best threshold here, e.g., 0.40 or 0.50 depending on your run]. This represents the best balance between Precision and Recall for this dataset.
Which metric matters more here? For this classification task, Recall is more important because missing a true positive (false negative) is more costly than raising a false alarm. For example, if the model predicts whether a student is at risk of not being placed, failing to identify them (low Recall) means they won’t receive needed support. False positives (predicting risk when the student is actually fine) are less harmful, since they only trigger extra attention.


Threshold adjustment:
	To optimise Recall, we would lower the threshold (e.g., from 0.50 to 0.40 or 0.30).
	Cost: Precision will drop, meaning more false positives. The model will flag more students as “at risk,” some of whom are not.
	This trade off is acceptable when catching every true case is more critical than avoiding false alarms.

    
AUC meaning: The Area Under the ROC Curve (AUC) measures the model’s ability to separate the two classes across all thresholds.
	An AUC of 1.0 means perfect separation — every positive is ranked above every negative.
	This indicates the model has excellent discriminative power on this dataset.
•  Comparison Table: | Model | Precision | Recall | AUC | |----------------------|-----------|--------|-----| | Baseline (C=1.0) | [your values] | [your values] | [your values] | | Regularized (C=0.01) | [your values] | [your values] | [your values] |
•  Interpretation:
	The baseline model (C=1.0) uses standard regularization.
	The second model (C=0.01) applies stronger L2 penalty, shrinking coefficients more aggressively.
	This often reduces variance and prevents overfitting, but can also lower Recall or Precision depending on the dataset.
	The AUC shows how well each model separates the classes overall.

    
•  Key takeaway: Stronger regularization (smaller C) tends to flatten coefficient magnitudes, making the model more conservative. This can improve generalization but may sacrifice sensitivity (Recall) or specificity (Precision). The choice depends on whether avoiding false negatives or false positives is more critical for your task.
Bootstrap Reliability of AUC Difference
We compared the baseline logistic regression (C = 1.0) against the strongly regularized model (C = 0.01) using n = 500 bootstrap samples drawn from the test set. For each sample, we computed the AUC difference:
Δ"AUC"=〖"AUC" 〗_(C=1.0)-〖"AUC" 〗_(C=0.01)
Results:
	Mean AUC difference: [insert your computed mean value]
	95% Confidence Interval: ([insert 2.5th percentile], [insert 97.5th percentile])
Interpretation:
	If the 95% CI excludes zero, the baseline model’s advantage is likely consistent across different data samples.
	If the 95% CI includes zero, the difference may not be reliable, meaning the stronger regularization could perform similarly on some resampled test sets.

