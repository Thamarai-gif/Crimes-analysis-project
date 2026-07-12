Decision Tree Classifier (Unconstrained Depth)
Training vs Test Accuracy:
	Training Accuracy: [insert your computed value]
	Test Accuracy: [insert your computed value]
Signs of Overfitting: The decision tree shows overfitting if training accuracy is very high (close to 1.0) while test accuracy is significantly lower. This happens because the tree, with no depth limit, keeps splitting until it perfectly memorizes the training data — but those splits don’t generalize well to unseen data.
Why Decision Trees are High Variance Models: Decision trees are described as high variance because they fit the training data greedily at each split without revisiting earlier decisions. Each split is chosen to maximize immediate purity, but once made, it cannot be undone. This greedy process makes trees highly sensitive to small changes in the data: a slightly different training set can produce a very different tree structure. As a result, unconstrained trees often capture noise and idiosyncrasies in the training set, leading to poor generalization.


Decision Tree with Controlled Depth
Hyperparameters:
	max_depth = 5: Limits how deep the tree can grow. This reduces variance (overfitting risk) at the cost of some bias (less flexibility).
	min_samples_split = 20: Prevents splitting a node if fewer than 20 samples are present. This avoids splits that respond to noise in very small subsets.
Training vs Test Accuracy:
	Unconstrained Tree (max_depth=None):
	Training Accuracy: [insert your value]
	Test Accuracy: [insert your value]
	Shows a large gap → classic overfitting.
	Controlled Tree (max_depth=5, min_samples_split=20):
	Training Accuracy: [insert your value]
	Test Accuracy: [insert your value]
	Gap is smaller → better generalization.
Interpretation:
	The unconstrained tree memorized the training data, achieving near perfect training accuracy but much lower test accuracy.
	By limiting depth and requiring more samples per split, the controlled tree sacrifices some training accuracy but improves test accuracy.
	This demonstrates the bias variance trade off: adding constraints increases bias slightly but reduces variance, leading to more reliable performance on unseen data.



Splitting Criteria in Decision Trees
Formulas:
	Gini Impurity
Gini=1-∑_(i=1)^k▒p_i^2 
Where p_iis the proportion of samples belonging to class iin the node.
	Entropy (Information Gain)
Entropy=-∑_(i=1)^k▒p_i ⋅〖log⁡〗_2 (p_i)
Where p_iis again the proportion of samples belonging to class i.
Interpretation:
	A node with Gini = 0 means all samples belong to a single class (perfectly pure).
	Similarly, Entropy = 0 also indicates perfect purity.
	Higher values of Gini or Entropy mean the node is more mixed, containing samples from multiple classes.
Comparison in Practice:
	Both measures aim to find splits that maximize node purity.
	Gini tends to favor larger class probabilities and is slightly faster to compute.
	Entropy uses logarithms and is more sensitive to class imbalance.
	In most datasets, the difference in performance between Gini and Entropy is small, as seen in the test accuracies reported for both models.


Bagging in Random Forests
Random Forests use the bagging technique to reduce variance compared to a single deep decision tree. Bagging means that each tree is trained on a bootstrap sample of the training data — a random sample drawn with replacement, so some rows may appear multiple times while others are left out. In addition, at each split within a tree, only a random subset of features is considered (commonly √("number of features" )for classification). This randomness ensures that trees are diverse and not all driven by the same dominant predictors. The final prediction is obtained by averaging across all trees (majority vote for classification, mean for regression). By combining many high variance learners, the ensemble smooths out noise and idiosyncrasies of individual trees, leading to a model that generalizes better and is more stable than a single unconstrained decision tree.


🔹 Feature Removal Analysis (Random Forest)
We retrained the Random Forest with the 5 lowest importance features removed and compared its performance to the full model:
	Full model ROC AUC: [insert your value]
	Reduced model ROC AUC: [insert your value]
Interpretation:
	If the reduced model’s AUC is similar or higher, the removed features were likely uninformative and may have added noise. In this case, deploying a simpler, lower dimensional model is beneficial: it reduces inference cost, memory usage, and maintenance burden without sacrificing predictive power.
	If the reduced model’s AUC drops noticeably, then even low importance features were contributing marginally. Removing them would degrade performance, and the trade off must be weighed carefully.




