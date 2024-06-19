# Validation phase
Once the optimal prompt is found, the final step is to evaluate the performance of the classifiers and the LLM. This is done by using the [classification_report](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.classification_report.html) function from the sklearn.metrics module, version 1.4.1.post1, which  provides various metrics, such as precision, f1-score, support, and accuracy, but most importantly recall. In fact, recall is the metric used in the current project to compare the models and determine which one is more effective at screening titles.

## Classifiers
To ensure validity, a Random Classifier is first run to confirm that the other classifiers outperform random guessing, as they would otherwise be unsuitable for comparison. The Logistic Regression, Random Forest, Naive Bayes, and Support Vector Machine are tested with the Calibration set 2. A confusion matrix is calculated for each classifier as well as the recall score.

## Large Language Model
Llama3 8b is also tested with the Calibration set 2 using the prompt 3.1 and prompt 17. The confusion matrix and recall score are calculated for the LLM too.