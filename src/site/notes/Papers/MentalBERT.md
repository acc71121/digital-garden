---
{"dg-publish":true,"permalink":"/papers/mental-bert/","tags":["gardenEntry"]}
---

#BERT #MentalBERT #MentalRoBERTa 

Paper link: https://arxiv.org/pdf/2110.15621.pdf


## Dataset

![Pasted image 20240314103437.png](/img/user/Images/Pasted%20image%2020240314103437.png)

[[Datasets/SWMH\|SWMH]]
[[Datasets/Stress Annotated Dataset (SAD)\|Stress Annotated Dataset (SAD)]]



## Language Model Pretraining

We follow the standard pretraining protocols of BERT and RoBERTa with Huggingface’s Transformers framework (Wolf et al., 2020). These two models work in a bidirectional manner, and we follow their mechanism and adopt the same loss of masked language modeling during pretraining. We use the base network architecture for both models. The BERT model we use is base uncased, which is 12-layer, 768-hidden, and 12-heads and has 110M parameters. For the pretraining of RoBERTabased MentalBERT, we apply the dynamic masking mechanism that converges slightly slower than the static masking. Instead of the domain-specific pretraining (Gu et al., 2020) that trains language models from scratch, we adopt the training scheme similar to the domain-adaptive pretraining (Gururangan et al., 2020) that continues the pretraining in specific downstream domains. Specifically, we start the training of language models from the checkpoint of original BERT and RoBERTa. In this way, we can utilize the learned knowledge from the general domain and save computing resources, and continued pretraining makes the model adaptive to the target domain of mental health.

## Pretraining Corpus 

We collect our pretraining corpus from Reddit, an anonymous network of communities for discussion among people of similar interests. Focusing on the mental health domain, we select several relevant subreddits (i.e., Reddit communities that have a specific topic of interest) and crawl the users’ posts. We do not collect user profiles when collecting the pretraining corpus, even though those profiles are publicly available. The selected mental health-related subreddits include “r/depression”, “r/SuicideWatch”, “r/Anxiety”, “r/offmychest”, “r/bipolar”, “r/mentalillness/”, and “r/mentalhealth”. Eventually, we make the training corpus with a total of 13,671,785 sentences.

## Downstream Task Fine-tuning 

We apply the pretrained MentalBERT and MentalRoBERTa in binary mental disorder detection and multi-class mental disorder classification of various mental disorders such as stress, anxiety, and depression. We fine-tune the language models in downstream tasks. Specifically, we use the embedding of the special token [CLS] of the last hidden layer as the final feature of the input text. We adopt the multilayer perceptron (MLP) with the hyperbolic tangent activation function for the classification model. We set the learning rate of the transformer text encoder to be 1e-05 and the learning rate of classification layers to be 3e-05. The optimizer is Adam.



## Results 

![Pasted image 20240314105251.png](/img/user/Images/Pasted%20image%2020240314105251.png)