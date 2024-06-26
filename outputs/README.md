# Results
The results are based on the code in the `scripts` folder of this repository.

The matrix containing the recall, the false negatives, and the false positives of all models tested during the validation phase is stored in the `Models_performances.csv` file.

# Training phase
During this phase, the Calibration Set 1 is utilized to train the models and to compare their generated labels with the human labels.

## Prompt optimization
The confusion matrices for the prompts described in this section are all displayed in the `llama3_8b.ipynb` notebook in the `prompt_optimization` folder of this repository.

For each prompt, a confusion matrix is generated to compare the model’s labels with the human title labels. Recall is subsequently calculated. After multiple attempts employing various prompt strategies, the optimal prompt is identified. Specifically, prompt 17 yields 0 False Negatives, resulting in a recall of 1.0. The confusion matrix for prompt 17 shows that 21 out of 298 papers are excluded, indicating that the model excludes approximately 7% of the papers. While this result is promising, further analysis is conducted to evaluate the potential for the model to exclude more papers, thereby increasing time and cost savings for researchers. Consequently, the model’s results from all previous prompts are re-evaluated, comparing them not against human labels for titles but against the human labels assigned after abstract screening. This approach helps determine whether the model is excluding relevant papers or those that would have been excluded later on during the abstract screening phase. Remarkably, prompt 1 already results in 0 False Negatives, achieving a recall of 1.0.

However, the prompt that achieves the highest number of excluded papers (True Negatives) while maintaining 0 False Negatives is prompt 3.1. Prompt 3.1 excludes 155 out of 298 papers, corresponding to a correct exclusion rate of 52%. This efficiency allows the model to significantly reduce the number of papers researchers need to review, resulting in substantial time and cost savings.

# Validation phase
During the validation phase, the Calibration Set 2 is utilized to test the models and to compare their generated labels with the human labels.

The confusion matrices for all the models described below are all displayed in the `compare_models.ipynb` notebook in the `validation_phase` folder of this repository.

## Classifiers
The labels made by the classifiers are compared to the human labels based on titles.

**Random Classifier**
The Random Classifier returns a recall of 0.5180, with 67 False Negatives and 86 True Negatives, which is essentially equivalent to random performance. In comparison, all classifiers evaluated below exhibit superior performance relative to the Random Classifier.

**Logistic Regression**
The Logistic Regression already demonstrates a strong performance, with a recall of 0.9928, 1 False Negative and 60 True Positives.

**Random Forest**
The Random Forest classifier returns a recall of 0.9281, with 10 False Negatives and 93 True Negatives.

**Naive Bayes**
The Naive Bayes classifier reaches a recall of 1.0, with 0 False Negatives and 37 True Negatives.

**Support Vector Machine (SVM)**
Additionally, the Support Vector Machine achieves a recall of 1.0, resulting in 0 False Negatives. However, the SVM excludes more papers compared to the Naive Bayes classifier. Specifically, the SVM excludes 57 out of 295 papers, making it the most effective classifier among those tested.

## Large Language Model
Prompt 17 is evaluated using the human labels based on titles, yielding 0 False Negatives and achieving a recall of 1.0. Consistently with the training phase, the number of True Negatives remains low, with a count of 16.

Prompt 3.1 is evaluated due to its higher true negative rate and its apparent lack of exclusion of relevant papers when compared to human labeling based on abstract screening. The recall for prompt 3.1 is 0.8889, with 1 False Negative and 182 True Negatives. Utilizing this prompt, Llama3 8B can exclude approximately 62% of the papers, although it misses one relevant paper.

# Human vs. machine error rate
[Wang et al. (2020)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6959565/) investigated human error rates (false exclusions + false inclusions) during the screening process of systematic reviews. Their study revealed that, depending on the question type, the average human error rate is 10.76%. 
In the current study, calculating the model's error rate is not relevant; instead, it is crucial to prioritize having zero false exclusions at the cost of more false inclusions. This approach aims to minimize the likelihood of type II errors, which involve erroneously excluding potentially relevant papers (Sedgwick, 2014). However, it is important to note that humans are also prone to type II errors, as illustrated in Figure 12.

In the current study, the model's labels were compared to human labels assigned by multiple reviewers collaboratively, as detailed in the `data` folder of this repository. Consequently, the human error within the two calibration sets is already minimized.

# Limitations
Although the findings of this study are promising, we considered some limitations for future implementations.

First, the datasets used for training and testing in the current study are relatively small (300 papers each) compared to the thousands of papers typically encountered by researchers. Future work should apply this procedure to larger datasets to evaluate the performance of the LLM with a more substantial volume of data.

Second, we used Llama3 8B due to its faster response time. However, Llama3 70B offers higher performance, while requiring more time to generate responses. Future research could investigate whether using Llama3 70B can reduce the error rate of the LLM.
Third, the classifiers used are machine learning models designed to minimize loss, aiming to reduce incorrect labels, including false positives, which are not the focus of this study. In contrast, the Large Language Model can be instructed to adopt a more conservative approach, thereby maximizing recall. Consequently, the two models are optimized differently, resulting in the observed variation in performance. Future projects should aim to compare models that are optimized for the same objective.

Lastly, we conducted a comparative analysis between errors made by humans and those made by the machine. However, we are not aware of any existing papers reporting the type II error rates of humans in paper screening for systematic reviews. Therefore, there is no benchmark to determine whether the type II error rate of the LLM aligns with human error rates or if humans perform better. Future studies could concentrate on calculating type II error rates in human screeners, similarly to the work of [Wang et al. in 2020](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6959565/).

# Take-home message
Large Language Models can be effectively used by researchers during the title screening phase to reduce both time and financial costs. Through prompt engineering, LLMs can be tailored to operate conservatively, ensuring that no pertinent papers are inadvertently excluded, thereby enhancing their usability.
Finally, it is essential to consider the trustworthiness of Large Language Models, since they can produce non-factual information, and their reasoning process is often opaque. Consequently, reliance solely on these models should be approached with caution. For this reason, we suggest using them exclusively in the preliminary screening phase, rather than in the more critical stages of abstract and full-text screening, which necessitate greater human involvement.