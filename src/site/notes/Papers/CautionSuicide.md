---
{"dg-publish":true,"permalink":"/papers/caution-suicide/","tags":["gardenEntry"]}
---

#GRU 

Paper link: https://arxiv.org/pdf/2401.01023.pdf

2 Jan 2024

Thus, in this paper, we proposed a novel, simple deep learning model to detect suicide ideation and a chatbot framework that can utilize such a model for early detection of suicide ideation. The contributions of this paper can be summarized as follows:

- A novel simple deep gate recurrent unit-based model to detect suicidal ideation in digital text content. 
- A chatbot-based framework that utilizes the proposed model to investigate and detect suicide ideation for users of the chatbot. In addition, the framework can provide additional reports for medical providers or corresponding authorities based on the issuer and the chatbot’s purpose. 
-  Provide open questions and limitations researchers can address to employ the proposed framework for different applications and targets.




### Dataset

[[Datasets/Suicide and Depression Detection\|Suicide and Depression Detection]]

The dataset consists of a collection of posts from subreddits of the Reddit platform, including ”SuicideWatch” and ”Depression” subreddits. The collected posts from SuicideWatch are labeled as suicide. The collected posts from the depression subreddit are labeled as depression. The non-suicide posts are collected from r/teenagers. The dataset consists of 233,337 data samples, where 117,301 are labeled as suicide and 116,039 are labeled as non-suicide.

The data has been preprocessed, including removing_stop_words, removing_punctuation, removing_emails, removing_HTML_tags, removing_special_characters, and removing_accented_characters. Then, the data was tokenized using Keras text preprocessing tokenizer, setting the number of words to 100,000. Then, the tokenized text has been converted to numeric sequences with post padding. Then, for model training and testing, the data labels have been coded as class 0 and class 1 for suicide and non-suicide, respectively. The data has been split to train, validate, and test the dataset with ratios 50%, 20%, and 30%, respectively.


