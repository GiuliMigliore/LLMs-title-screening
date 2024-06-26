# The application of LLM prompt engineering to optimize title screening 

A code repository for:
>The application of LLM prompt engineering to optimize title screening

When researchers have to write a systematic review or meta-analysis, they need to search in detail for relevant papers to ensure a comprehensive overview. After deciding on the review question, researchers must systematically search for all relevant literature. They then proceed through two main phases: 

- title-abstract screening, where irrelevant papers are excluded

- full-text screening of the remaining papers 

These two steps ultimately narrow down the selection to the most relevant studies for the review.

It has been demonstrated in the literature that a titles-first screening strategy can be highly effective for systematic reviews, discarding a big portion of irrelevant papers early, reducing the amount of abstracts to read later. 

The inclusion and exclusion criteria used for title screening are pretty straightforward, meaning that this process can potentially be automated. Large Language Models (LLMs) could be the ideal automated assistants for screening titles. 

This way there would be three phases:

- title screening (automated)

- abstract screening (human input)

- full-text screening (human input)

## Objectives
This project has two main objectives: first, to compare various prompt techniques and identify the one that elicits the most effective results from the LLM; second, to evaluate the performance of the LLM against simpler classifiers to determine if the LLM more effectively filters out irrelevant papers compared to simpler statistical methods. 

The models should exclude a significant portion of papers, while ensuring that no relevant paper is excluded. False inclusions are acceptable at this point, as this is merely the initial stage. All papers identified as relevant will undergo a secondary review that includes reading their abstracts. 
If this method proves effective, it can serve as an initial stage to reduce the number of papers that need to be screened manually.

# Design
The entire design of the current project is illustrated in the following diagram. 

![Simulation design](https://github.com/GiuliMigliore/LLMs-title-exclusion/assets/154629511/dfecd8fa-f577-4aea-ae31-c52bf27bffb1)

The study is divided into two phases: a training phase and a testing phase. For the training phase, the Calibration Set 1 is utilized. The dataset is first preprocessed, as detailed in the "data" folder of the current repository. Next, the preprocessed dataset is used to conduct multiple trials on the LLM with different prompts to identify the optimal prompt. The ideal prompt should result in zero False Negatives when the LLM's labels are compared to the human labels. To validate the efficacy of the LLM method, the preprocessed Calibration Set 1 is also used to train simpler classifiers, ensuring that the LLM produces more reliable labels compared to less complex methods. If the LLM does not outperform the simpler classifiers, it would not be justifiable to use the significant computational resources required for a LLM.

Once the optimal prompt is identified, the testing phase begins. The Calibration Set 2 is used to evaluate the simpler classifiers and to test the LLM with the selected optimal prompt. The performance of the LLM is then compared to that of the simpler classifiers to determine whether the LLM can screen titles more effectively than traditional methods.

# Training phase

## Classifiers
The models trained for the comparison include Logistic Regression, Random Forest, Naive Bayes, and Support Vector Machine, all implemented using version 1.5.0 of the [scikit-learn library](https://scikit-learn.org/stable/).
A [tf-idf vectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html) is employed to convert the text in the title column into numerical vectors before inputting it into the simple classifiers. 

## Large Language Model
The LLM used for this project is Meta’s [Llama 3](https://ai.meta.com/blog/meta-llama-3/). It is an open-source pre-trained language model available in the 8B or 70B parameters version. The model was released in April 2024, reaching state-of-the-art performance. Since the 70B version takes longer to process inputs and deliver outputs, the [8B version](https://ollama.com/library/llama3:8b) is utilized for the current project. 

The [Ollama API](https://github.com/ollama/ollama/blob/main/docs/api.md) is used to run the model. Ollama supports [Nvidia GPUs](https://github.com/ollama/ollama/blob/main/docs/gpu.md) with compute capability 5.0+. 

Vectorization is not necessary for Llama, since the model already vectorizes the text by itself.

# Contents
Folders:
- `data` - A folder containing information about the data and the data preprocessing script.
- `scripts` - A folder containing the scripts for the prompt optimization phase and the validation phase of this simulation study.
- `outputs` - Contains information about the output and the final performance of all the model used during the simulation.

# Licence
The content in this repository is published under the MIT license.

# Contact
For any questions or remarks, please contact the corresponding author: 
 
> Giulia Migliore: [@GiuliMigliore](https://github.com/GiuliMigliore)

For questions regarding this repository, use the
[issue](https://github.com/GiuliMigliore/LLMs-title-exclusion/issues) tab.

# Acknowledgments

We would like to thank the contributors to this project.

First, [Rens van de Schoot](https://github.com/Rensvandeschoot) provided guidance and support throughout the course of this project. 

Second, [Jelle Teijema](https://github.com/jteijema) gave insightful feedback and reflections on critical aspects of the project, which were instrumental in shaping this work.

Third, Bruno Messina Coimbra assisted in crafting the step-by-step reasoning prompts. His detailed explanations of the reasoning behind specific labeling decisions in the human dataset were crucial in establishing a connection between the human and machine labeling processes.