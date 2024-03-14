---
{"dg-publish":true,"permalink":"/papers/depression-emo/","tags":["gardenEntry"]}
---

#SVM   #Light_GBM #XGBoost  #GAN-BERT #BERT #BART #DeBERTa

Paper link: https://arxiv.org/pdf/2401.04655.pdf

9 Jan 2024

### Abstract

Abstract Emotions are integral to human social interactions, with diverse responses elicited by various situational contexts. Particularly, the prevalence of negative emotional states has been correlated with negative outcomes for mental health, necessitating a comprehensive analysis of their occurrence and impact on individuals. In this paper, we introduce a novel dataset named DepressionEmo designed to detect 8 emotions associated with depression by 6037 examples of long Reddit user posts. This dataset was created through a majority vote over inputs by zero-shot classifications from pre-trained models and validating the quality by annotators and ChatGPT, exhibiting an acceptable level of interrater reliability between annotators. The correlation between emotions, their distribution over time, and linguistic analysis are conducted on DepressionEmo. Besides, we provide several text classification methods classified into two groups: machine learning methods such as SVM, XGBoost, and Light GBM; and deep learning methods such as BERT, GAN-BERT, and BART. The pretrained BART model, bart-base allows us to obtain the highest F1- Macro of 0.76, showing its outperform compared to other methods evaluated in our analysis. Across all emotions, the highest F1-Macro value is achieved by suicide intent, indicating a certain value of our dataset in identifying emotions in individuals with depression symptoms through text analysis. 

## Task Description:

As a multilabel classification problem, the task is to detect a list of emotions E = e$_{1}$, e$_{2}$, ..., e$_{N}$ occurring in a given text t, which can contain from 1 to N emotions, but it is not empty. In DepressionEmo, there are 8 emotions, including anger, cognitive dysfunction, emptiness, hopelessness, loneliness, sadness, suicide intent, and worthlessness.

### Dataset

To create DepressionEmo, a web crawler to collect user posts from different subreddits was developed (r\\depression , r\\Depressed partners, r\\loneliness r\\suicide, and r\\suicide_watch)

Texts from subreddit r\\depression and r\\Depressed partners are considered as depressed related texts. We found out some specific keywords and common words from subreddit r\\depression and r\\Depressed partners to gathered examples from r\\loneliness, r\\suicide, and r\\suicide_watch to include in our dataset.

We exploited various common pronouns like **I, my, me, im, i’m, am**, absolute terms such as **everything, nothing, always, anymore, any**, as well as negative expressions like **no, never**. Additionally, more specific keywords, including **lonely, loneliness, alone, suicide, fuc*, shi*, feel, depression, can’t sleep, no sleep, feel exhausted, no motivation** were considered for collecting data from subreddits r\\loneliness, r\\suicide, and r\\suicide_watch.

From around **8k** extracted examples, we reduced them to **6k** examples by considering post length with **3** only **256** tokens and eliminating the examples without emotions or providing only mental health suggestions for users.

##### Data Preprocessing

Each user post is comprised of six fields: **id, title, post, text, upvotes, date, emotions**. The text field is constructed from the concatenation of **title and post**, using the delimiter **###**, then detect depression emotions in it by using the majority vote from #zero-shot classifications models. To guarantee the appropriate length of sequences for training models, any text surpassing 240 tokens or falling below five tokens is omitted. This ensures that the models concentrate on training with a maximum of 256 tokens. In addition, we have done some **basic preprocessing steps** such as #lemmatization and #removing_stop_words and #removing_punctuation. We have no other preprocessing steps to keep the data original and to see how models deal with it.


### Annotation

Categorize eight emotions **(anger, cognitive dysfunction, emptiness, hopelessness, loneliness, sadness, suicide intent, worthlessness)** within the text fields by leveraging #zero-shot classification outcomes from pretrained models. Addressing this as a multilabel classification challenge, we determine the final emotions through a majority vote. 

We use 4 pre-trained models:

- model1 = MoritzLaurer/mDeBERTa-v3-base -mnli-xnli, 
- model2 = MoritzLaurer/DeBERTa-v 3-large-mnli-fever-anli-ling-wanli
- model3 = cross-encoder/nli-deberta-v3-small
- model4 = facebook/bart-large-mnli)

For each pre-trained model, labels with scores greater than 0.5 are considered, and the corresponding emotions are added as the output from each model. Several chosen examples are presented in Table:

| Date and Time       | (Title + Post)                                                                                                                                                                                                                              | Emotions                                             |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| 2023-08-01 01:08:30 | Will it ever end? ### The day to day loathing. The ideation of it being over. The sleeping through the day. The indecisiveness. The fear of being in public. The shame carried with depression... I am ashamed of how pathetic I am.        | hopelessness, sadness, worthlessness, suicide intent |
| 2019-01-05 10:19:56 | Does anyone else get the feeling that life is just passing them by? ### I feel like time is moving too fast for me to seize it. I have no idea where 2018 went. And it was the same for 2017 and its gonna be similar for this year I fear. | sadness, hopelessness                                |

The annotation quality must be evaluated based on the inter-rater reliability between annotators. We follow a similar approach but for checking the annotation quality. We asked 3 Ph.D. students to annotate 100 random examples and then measure their consensus with our dataset labeling by several coefficients [[Measures/Bennett, Alpert and Goldstein’s S (S)\|Bennett, Alpert and Goldstein’s S (S)]], [[Measures/Fleiss’ kappa\|Fleiss’ kappa]] or multi-kappa (κ$_{f}$), [[Measures/Krippendorff’s alpha (α)\|Krippendorff’s alpha (α)]], and [[Measures/Scott’s Pi (π)\|Scott’s Pi (π)]]. These are frequently employed statistical measures for assessing the consistency among multiple evaluators when evaluating items across various categories. Below are the scales associated with each coefficient:

-  α: The scale spans from -1 to 1, where 1 indicates perfect agreement, 0 indicates no agreement beyond chance, and negative values indicate disagreement. α$_{n}$ refers to alpha nominal that is used for nominal data.
-  π: The calculation is expressed as π = P$_{0}$−P$_{e}$/(1− P$_{e}$), where P$_{0}$ represents the observed proportion of agreement and P$_{e}$ denotes the expected proportion by chance. The scale ranges from a minimum value of −P$_{e}$/(1−P$_{e}$) (when P$_{0}$ equals 0) to a maximum value of 1 (when P$_{0}$ equals 1)
-  S: The scale ranges from −1 to 1. A perfect agreement between the two raters is denoted by 1, while −1/(n − 1) is assigned when the observed agreement proportion P$_{0}$ equals 0. The minimum value of −1 is associated with the number of categories n being equal to 2, and this value approaches 0 as n increases 
-  κf : The coefficient’s author proposed that values ≤ 0 signify no agreement, 0.01–0.20 signify none to slight agreement, 0.21–0.40 signify fair agreement, 0.41–0.60 signify moderate agreement, 0.61–0.80 signify substantial agreement, and 0.81–1.00 signify almost perfect agreement. It is used for the agreement between 3 or more raters

 The information presented in Table illustrates a fair agreement between annotators and dataset labels, with most coefficient values falling between 0.5 and 0.99. We consider these outcomes acceptable, considering the high cost of manual annotation and the challenges associated with multilabel classification. Additionally, in this context, the average value for sadness is the highest, while emptiness has the lowest average value.

Table The inter-rater reliability between annotators and dataset labels:

| Emotion               | α$_{n}$ | π    | S    | κ$_{f}$ |
| --------------------- | ------- | ---- | ---- | ------- |
| anger                 | 0.65    | 0.65 | 0.68 | 0.65    |
| cognitive dysfunction | 0.94    | 0.93 | 0.97 | 0.94    |
| emptiness             | 0.51    | 0.54 | 0.51 | 0.51    |
| hopelessness          | 0.64    | 0.64 | 0.67 | 0.64    |
| loneliness            | 0.62    | 0.64 | 0.62 | 0.61    |
| sadness               | 0.97    | 0.97 | 0.98 | 0.97    |
| suicide intent        | 0.92    | 0.91 | 0.94 | 0.92    |
| worthlessness         | 0.92    | 0.91 | 0.91 | 0.91    |


#### Comparison with Other Depression Datasets

| Dataset                  | Train/Val/Test | Source            | Text Len. | No. Labels | Vocab. Size |
| ------------------------ | -------------- | ----------------- | --------- | ---------- | ----------- |
| [[Datasets/BLDC\|BLDC]]                 | 60k            | Twitter           | 6.50      | 2          | 13,951      |
| [[Datasets/DATD\|DATD]]                 | 4650           | Twitter           | 11.24     | 2          | 9,595       |
| [[Datasets/DepressionEmoDataset\|DepressionEmoDataset]] | 4225/906/906   | Reddit            | 103.97    | 8          | 18,192      |
| [[Datasets/GoEmotions\|GoEmotions]]           | 58k            | Reddit            | 12.99     | 27         | 68,937      |
| [[Datasets/MLDC\|MLDC]]                 | 57k            | Twitter           | 4.93      | 3          | 12,647      |
| [[Datasets/SNCDL\|SNCDL]]                | 1850           | Reddit            | 82.98     | 2          | 11,869      |
| [[Datasets/8datasets\|8datasets]]            | 3600           | Forums and Reddit | -         | 2          | -           |
| [[Datasets/Depression dataset\|Depression dataset]]   | 1893K          | Reddit            | -         | 2          | -           |


### Results

| Method    | Type | F1-Mac   | P-Mac    | R-Mac    | F1-Mic   | P-Mic    | R-Mic    | Avg.     |
| --------- | ---- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| SVM       | ML   | 0.47     | **0.72** | 0.41     | 0.61     | **0.77** | 0.51     | 0.58     |
| Light GBM | ML   | 0.58     | 0.48     | 0.80     | 0.65     | 0.52     | 0.86     | 0.65     |
| XGBoost   | ML   | 0.59     | 0.63     | 0.56     | 0.66     | 0.69     | 0.63     | 0.63     |
| GAN-BERT  | DL   | 0.70     | 0.69     | 0.72     | 0.75     | 0.73     | 0.77     | 0.73     |
| BERT      | DL   | 0.74     | 0.72     | 0.77     | 0.79     | 0.76     | 0.83     | 0.77     |
| BART      | DL   | **0.76** | 0.70     | **0.81** | **0.80** | 0.74     | **0.86** | **0.78** |
Table: The results on the test set by different text classification methods.

**ML**: Machine Learning,
**DL**: Deep Learning,
**F1-Mac**: F1 Macro, 
**P-Mac**: Precision Macro, 
**Re-Mac**: Recall Macro,
**F1-Mic**: F1 Micro, 
**P-Mic**: Precision Micro, 
**Re-Mic**: Recall Micro,
**Avg**: Average

| Emotion               | F1-Mac | P-Mac | R-Mac | F1-Mic | P-Mic | R-Mic | Avg. |
| --------------------- | ------ | ----- | ----- | ------ | ----- | ----- | ---- |
| anger                 | 0.78   | 0.78  | 0.78  | 0.79   | 0.79  | 0.79  | 0.79 |
| cognitive dysfunction | 0.67   | 0.66  | 0.69  | 0.78   | 0.78  | 0.78  | 0.73 |
| emptiness             | 0.76   | 0.76  | 0.78  | 0.77   | 0.77  | 0.77  | 0.77 |
| hopelessness          | 0.73   | 0.81  | 0.70  | 0.80   | 0.80  | 0.80  | 0.77 |
| loneliness            | 0.81   | 0.82  | 0.82  | 0.81   | 0.81  | 0.81  | 0.81 |
| sadness               | 0.67   | 0.77  | 0.64  | 0.82   | 0.82  | 0.82  | 0.76 |
| suicide intent        | 0.85   | 0.85  | 0.86  | 0.89   | 0.89  | 0.89  | 0.87 |
| worthlessness         | 0.75   | 0.76  | 0.76  | 0.75   | 0.75  | 0.75  | 0.75 |
Table: The test set’s result by emotions on BART