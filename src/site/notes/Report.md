---
{"dg-publish":true,"permalink":"/report/","tags":["gardenEntry"]}
---


## [[Papers/! DepressionEmo\|! DepressionEmo]]

#### Multilabel classification problem@_

Used 4 pre-trained models for transfer learning:

- model1 = MoritzLaurer/mDeBERTa-v3-base -mnli-xnli, 
- model2 = MoritzLaurer/DeBERTa-v 3-large-mnli-fever-anli-ling-wanli
- model3 = cross-encoder/nli-deberta-v3-small
- model4 = facebook/bart-large-mnli)


| Date and Time       | (Title + Post)                                                                                                                                                                                                                              | Emotions                                             |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| 2023-08-01 01:08:30 | Will it ever end? ### The day to day loathing. The ideation of it being over. The sleeping through the day. The indecisiveness. The fear of being in public. The shame carried with depression... I am ashamed of how pathetic I am.        | hopelessness, sadness, worthlessness, suicide intent |
| 2019-01-05 10:19:56 | Does anyone else get the feeling that life is just passing them by? ### I feel like time is moving too fast for me to seize it. I have no idea where 2018 went. And it was the same for 2017 and its gonna be similar for this year I fear. | sadness, hopelessness                                |

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

#### Results

| Method    | Type | F1-Mac   | P-Mac    | R-Mac    | F1-Mic   | P-Mic    | R-Mic | Avg. |
| --------- | ---- | -------- | -------- | -------- | -------- | -------- | ----- | ---- |
| SVM       | ML   | 0.47     | **0.72** | 0.41     | 0.61     | **0.77** | 0.51  | 0.58 |
| Light GBM | ML   | 0.58     | 0.48     | 0.80     | 0.65     | 0.52     | 0.86  | 0.65 |
| XGBoost   | ML   | 0.59     | 0.63     | 0.56     | 0.66     | 0.69     | 0.63  | 0.63 |
| GAN-BERT  | DL   | 0.70     | 0.69     | 0.72     | 0.75     | 0.73     | 0.77  | 0.73 |
| BERT      | DL   | 0.74     | 0.72     | 0.77     | 0.79     | 0.76     | 0.83  | 0.77 |
| BART      | DL   | **0.76** | 0.70     | **0.81** | **0.80** | 0.74     |       |      |


## [[Papers/* Multi-Task Learning for Depression Detection in Dialogs\|* Multi-Task Learning for Depression Detection in Dialogs]]

## [[Papers/Capturing Long Context in Mental Health\|Capturing Long Context in Mental Health]]

## [[Papers/MentaLLaMA\|MentaLLaMA]]

## [[Papers/! Read, Diagnose and Chat\|! Read, Diagnose and Chat]]

## [[Papers/!! A Time-Aware Transformer Based Model forSuicide Ideation Detection on Social Media\|!! A Time-Aware Transformer Based Model forSuicide Ideation Detection on Social Media]]

## [[Papers/!! Hierarchical BERT for  Suicide Risk\|!! Hierarchical BERT for  Suicide Risk]]


# #Offtop

## [[Papers/! An Automated Tool to Detect Suicidal Susceptibility from Social Media Posts\|! An Automated Tool to Detect Suicidal Susceptibility from Social Media Posts]]

## [[Papers/CautionSuicide\|CautionSuicide]]
## [[Papers/Balanced and Explainable Analysis\|Balanced and Explainable Analysis]]


