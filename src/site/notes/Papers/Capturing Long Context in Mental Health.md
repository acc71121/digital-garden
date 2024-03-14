---
{"dg-publish":true,"permalink":"/papers/capturing-long-context-in-mental-health/"}
---

#BERT #RoBERTa #XLNet #Longformer #ChatGPT #MentalBERT #MentalRoBERTa #MentalXLNet #MentalLongformer

Paper Link: https://arxiv.org/pdf/2304.10447.pdf

20 Apr 2023

## Dataset


![Pasted image 20240314101945.png](/img/user/Images/Pasted%20image%2020240314101945.png)


We introduce two transformer networks for long documents and domain-specific pretraining to continue the pretraining in the mental healthcare domain. Table 1 summarizes the learning objectives and sequence lengths of existing pretrained models for mental healthcare and models trained in this paper.


![Pasted image 20240314102932.png](/img/user/Images/Pasted%20image%2020240314102932.png)

## Results of mental health classification

![Pasted image 20240313104317.png](/img/user/Images/Pasted%20image%2020240313104317.png)

**Zero-shot** ChatGPT We compare our models with zero-shot ChatGPT referring to Yang et al. (2023). ChatGPT$_{ZS}$ uses ChatGPT as the inference engine with simple manual prompts, ChatGPT$_{V}$ , ChatGPT$_{N_sen}$, and ChatGPT$_{N_emo}$ inject distant supervision signals from lexicons, i.e., NRC EmoLex sentiments (Mohammad and Turney, 2013), VADER sentiments (Hutto and Gilbert, 2014), and SenticNet (Cambria et al., 2022), to the prompts. ChatGPT$_{CoT}$ and ChatGPT$_{CoT\_emo}$ utilize the Chain-of-Thought method (Wei et al., 2022) and its enhancement with emotion (Amin et al., 2023).

![Pasted image 20240313104357.png](/img/user/Images/Pasted%20image%2020240313104357.png)

Table: Long-range ability analysis on UMD and CLPsych15


## Reference

[[Papers/MentalBERT\|MentalBERT]]

## #Source_code 

https://huggingface.co/AIMH
