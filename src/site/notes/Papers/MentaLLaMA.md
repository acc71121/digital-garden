---
{"dg-publish":true,"permalink":"/papers/menta-l-la-ma/","tags":["gardenEntry"]}
---

#LLM

Paper Link: https://arxiv.org/pdf/2309.13567.pdf

4 Feb 2024


## Dataset

![Pasted image 20240313164539.png](/img/user/Images/Pasted%20image%2020240313164539.png)

#### IMHI DATASET 

This section introduces the construction process of the IMHI dataset. The process mainly involves 4 procedures: raw data collection, explanation generation via ChatGPT, evaluation for the generated explanations, and instruction construction. 

##### Raw Data Collection 
The raw data is collected from 10 existing mental health analysis datasets from multiple social media data sources, including Reddit, Twitter, and Short Message Service (SMS) texts. These datasets are also with high-quality annotations, which are important resources for explanation generation and AIGC evaluation. More statistics of the collected raw data are shown below and in Table 1.
##### Binary mental health detection
This task aims to detect symptoms of one mental health condition, where each social media post is annotated with a binary label. We select two datasets for depression symptom detection: Depression_Reddit (DR) and CLPsych15 (CLP). We also utilize Dreaddit, a dataset for stress detection, and a loneliness symptom detection dataset. 

##### Multi-class mental health detection
This task aims to identify symptoms of one mental health condition from a given list of multiple mental health conditions, which are normally modeled as a multi-class single-label classification task. We select T-SID and SWMH datasets for this task, including symptoms of depression, PTSD, anxiety, etc.
##### Mental health cause/factor detection 
With a post showing a mental health condition, this task aims to assign a label to the post for a possible cause/factor leading to the mental health condition from a given causes/factors list. Common causes include social relationships, medication, work pressure, etc. We select a stresscause detection dataset SAD and a depression/suicide cause detection dataset CAMS. 

##### Mental risk/wellness factors detection
This task dives deep into the social or mental factors behind mental health conditions and aims to identify psychological risk/wellness factors from social media posts, which is also modeled as a classification task to detect the existence of certain factors. We select IRF, an annotated dataset for interpersonal risk factors of mental disturbance. Another dataset called MultiWD is also collected, which is developed for analyzing mental wellness dimensions from psychological models.

#### Explanation Generation with ChatGPT 

Though rich data sources with high-quality classification annotations are available, it lacks open-source data that provides detailed and reliable explanations for the annotations. Therefore, we leverage ChatGPT to generate explanations for the collected samples, which is proven a reliable LLM in interpretable mental health analysis. 
Firstly, we ask the domain experts to manually write 1 task-specific instruction and 35 explanation examples for each of the tasks in 10 collected datasets. The expert-written explanations lead to a gold explanation set G with 350 samples. To facilitate model training and evaluation, all expert-written explanations are based on the following template: 

\[label] Reasoning: \[explanation] 

where \[label] and \[explanation] denote the classification annotation and the corresponding explanation content. Secondly, for each dataset, we randomly sample 2 explanations from G for each class, and include them as few-shot examples in the prompt. To further enhance the generation quality, we include supervised annotations from the raw datasets. Thirdly, we utilize task-specific instruction, few-shot expert-written examples, and the assigned annotation for the target post to construct the prompt for ChatGPT explanation generation. An example of the constructed prompt for the dataset DR is shown in Figure 2, and the prompts for other datasets are presented in Table:
![Pasted image 20240314110322.png](/img/user/Images/Pasted%20image%2020240314110322.png)

## Results

![Pasted image 20240314103735.png](/img/user/Images/Pasted%20image%2020240314103735.png)
## Reference

[[Papers/Interpretable Mental Health Analysis\|Interpretable Mental Health Analysis]]

## #Source_code

https://github.com/SteveKGYang/MentalLLaMA