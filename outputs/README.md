# Results
The results are based on the code in the "scripts" folder of the current repository.

Finally, a matrix called "Models_performances.csv" is created to compile the recall, the False Negatives, and the False Positives of all models tested during the validation phase, facilitating an easy comparison of their performances.

# Training phase
During this phase, the Calibration Set 1 is utilized to train the models and to compare their generated labels with the human labels.

## Prompt optimization
The confusion matrices for the prompts described in this section are all displayed in the "llama3_8b.ipynb" notebook in the "prompt_optimization" folder of the current repository.

For each prompt, a confusion matrix is generated to compare the model’s labels with the human title labels. Recall is subsequently calculated. After multiple attempts employing various prompt strategies, the optimal prompt is identified. Specifically, prompt 17 yields 0 False Negatives, resulting in a recall of 1.0. The confusion matrix for prompt 17 shows that 21 out of 298 papers are excluded, indicating that the model excludes approximately 7% of the papers. While this result is promising, further analysis is conducted to evaluate the potential for the model to exclude more papers, thereby increasing time and cost savings for researchers. Consequently, the model’s results from all previous prompts are re-evaluated, comparing them not against human labels for titles but against the human labels assigned after abstract screening. This approach helps determine whether the model is excluding relevant papers or those that would have been excluded later on during the abstract screening phase. Remarkably, prompt 1 already results in 0 False Negatives, achieving a recall of 1.0.

However, the prompt that achieves the highest number of excluded papers (True Negatives) while maintaining 0 False Negatives is prompt 3.1. Prompt 3.1 excludes 155 out of 298 papers, corresponding to a correct exclusion rate of 52%. This efficiency allows the model to significantly reduce the number of papers researchers need to review, resulting in substantial time and cost savings.

# Validation phase
During the validation phase, the Calibration Set 2 is utilized to test the models and to compare their generated labels with the human labels.

The confusion matrices for all the models described below are all displayed in the "compare_models.ipynb" notebook in the "validation_phase" folder of the current repository.

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

# Discussion and conclusions
The Naive Bayes classifier, the Support Vector Machine, and Llama3 8B with prompt 17 each achieve a recall of 1.0. However, these models differ in the number of papers they correctly exclude: Naive Bayes excludes 37 papers, the SVM excludes 57 papers, and Llama3 8B excludes 16 papers. Based on the number of True Negatives, the Support Vector Machine is the most effective model for screening papers based on titles for systematic reviews, as it can automatically exclude over 19% of the papers without missing any relevant paper, making it the safest choice.

In contrast, if the objective is to maximize the number of correctly excluded papers, Llama3 8B with prompt 3.1 is the best model, correctly excluding 182 papers with only one missed paper. Although the LLM makes isolated errors, it is important to consider whether these errors are comparable to those made by humans.

[Wang et al. (2020)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6959565/) investigated human error rates, comprising both false exclusions and false inclusions, during the screening process of systematic reviews. Their study revealed that, depending on the question type, the average human error rate was 10.76%. 
In the preliminary phase of the current study, calculating the model's error rate is not pertinent; instead, it is crucial to prioritize having more false inclusions while ensuring zero false exclusions. This approach aims to minimize the likelihood of type II errors, which involve erroneously excluding potentially relevant papers. However, it is important to note that humans are also prone to type II errors, as illustrated in the diagram below.

In the current study, the model's labels were compared to human labels assigned by multiple reviewers collaboratively, as detailed in the "data" folder of the current repository. Consequently, the human error within the two calibration sets is already minimized.

However, there are no published studies that report the type II error rates of humans in paper screening for systematic reviews. Therefore, there is no benchmark to determine whether the type II error rate of the LLM aligns with human error rates or if humans perform better.
Future studies could concentrate on calculating type II error rates in human screeners, similarly to the work of Wang et al. in 2020.

Furthermore, Llama3 8B was utilized in the current study due to its faster response time. However, [Llama3 70B](https://ollama.com/library/llama3:70b), despite requiring more time to generate responses, offers higher performance. Future research could investigate whether using Llama3 70B would reduce the error rates of the LLM to zero.

To assess the quality of prompts, either subjective or objective evaluations can be utilized. Subjective evaluations typically involve human reviewers and, despite potential inconsistencies, often result in higher quality assessments. In this project, a human-labeled dataset is used as a benchmark to assess the accuracy of the LLM-generated labels. On the other hand, objective evaluations, such as [BLEU](https://huggingface.co/spaces/evaluate-metric/bleu), [ROUGE](https://huggingface.co/spaces/evaluate-metric/rouge), [METEOR](https://github.com/numtel/meteor-benchmark-packages), and [BERTScore](https://huggingface.co/spaces/evaluate-metric/bertscore), provide more standardized measures but may lack precision due to their generalized nature. 

Finally, different prompts result in varying levels of performance quality in the model. In the current project, we have outlined best practices for creating high-quality prompts and described our successful approaches. However, it is essential to recognize that prompts can vary between users, implying that the quality of outputs depends on the individual crafting the prompt.