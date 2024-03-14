---
{"dg-publish":true,"permalink":"/papers/multi-task-learning-for-depression-detection-in-dialogs/"}
---


#hierarchical #MTL #STL #RNN #bi-LSTM #LSTM #PHQ-9

Paper link: https://arxiv.org/pdf/2208.10250.pdf

21 Jul 2022

**Abstract**

We hypothesize that depression and emotion can inform each other, and we propose to explore the influence of dialog structure through topic and dialog act prediction. We investigate a Multi-Task Learning (**MTL**) approach, where all tasks mentioned above are learned jointly with dialog-tailored hierarchical modeling. We experiment on the DAIC and DailyDialog corpora – both contain dialogs in English – and show important improvements over state-of-the-art on depression detection (at best 70.6% $F_{1}$), which demonstrates the correlation of depression with emotion and dialog organization and the power of **MTL** to leverage information from different sources.
### Model Architecture

==*One condition generally assumed for success within **MTL**, at least in **NLP**, is that the primary and auxiliary tasks should be related*==

The emotion-related task is thus a natural choice since it is linked to mental states. We hypothesize that depressive disorder can also affect how people interact with others during conversations. We thus take a first step toward linking dialog structure and depression by examining shallow signals: dialog acts and topics. In addition, since the information comes at different levels, we propose hierarchical modeling, from speech turns to documents

##### Baseline Model

Our basic model is a two-level recurrent network, similar to the one in [Cerisara, 2018](https://aclanthology.org/C18-1063.pdf) 
The input words are mapped to vectors using word embeddings from scratch. The first level (turn-level) takes the embeddings into a **bi-LSTM** network to obtain one vector for each turn. The second level (dialog-level) takes a sequence of turns into an **RNN** network, and the output is finally passed into a linear layer for depression prediction


![Pasted image 20240307141703.png](/img/user/Images/Pasted%20image%2020240307141703.png)

###### MTL Model

The **MTL** architecture is composed of shared hidden layers and task-specific output layers 
(see Fig. 1) and corresponds to the hard parameter sharing approach [Caruana, 1993, 1997; Ruder, 2017](https://www.cs.cornell.edu/~caruana/ml93.ps)
Since some auxiliary tasks are at the speech-turn level **(i.e., emotion, dialog act)** while others are annotated at the document level **(i.e., depression, topic)**, our architecture is hierarchical and arranges task-specific output layers (**MLP**) at two levels. Speech-turn level emotion and dialog act information can be learned in the turn-level **LSTM** network and transferred upwards to help depression and topic prediction. On the other hand, higher-level information can be backpropagated to update the network at the lower level. The loss is simply the sum of the losses for each task. When it comes to the **MTL** setting, we set equal weight for each task as the standard choice

### Datasets

##### DAIC-WOZ

This dataset is a subset of the DAIC corpus [Gratch et al., 2014](http://www.lrec-conf.org/proceedings/lrec2014/pdf/508_Paper.pdf) [see](https://dcapswoz.ict.usc.edu/)
It contains 189 sessions (one session is one dialog with avg. 250 speech turns) of two-party interviews between participants and Ellie – an animated virtual interviewer controlled by two humans. Table gives the partition of train (107), development (35), and test (47) sets. Originally, patients are associated with a score related to the ==Patient Health Questionnaire== (**PHQ-9**): a patient is considered depressive if **PHQ-9** ≥ 10 [Kroenke and Spitzer, 2002](https://www.scinapse.io/papers/1985329916)

|               | Train | Dev | Test |
| ------------- | ----- | --- | ---- |
| Depressed     | 77    | 23  | 33   |
| Non Depressed | 30    | 12  | 14   |
| Total         | 107   | 35  | 47   |
	Number of sessions (dialogs) in DAIC-WOZ

##### [[Datasets/DailyDialog\|DailyDialog]] 

This dataset [Li et al., 2017](https://aclanthology.org/I17-1099.pdf) contains 13, 118 two-party dialogs (with averaged 7.9 speech turns per dialog) for English learners [see](http://yanran.li/dailydialog), covering various topics from ordinary life to finance. Three expert-annotated information are provided: 7 emotions, 4 coarse-grain dialog acts, and 10 topics. We select this corpus due to its large size, two-level annotations and high quality. The train set contains > 87k turns for emotions and dialog acts and > 11k dialogs for topics. Detailed statistics are given in Tables at the end of the paper.

### Experimental setup

##### Baselines

<u>We compare our MTL results with</u>: 
(1) Majority class where the model predicts all positive; 
(2) Baseline single-task model (see Sec. 3); 
(3) State-of-the-art results on test set reported by [Mallol-Ragolta et al. (2019)](https://opus.bibliothek.uni-augsburg.de/opus4/frontdoor/deliver/index/docId/65784/file/2036.pdf) and [Xezonaki et al. (2020)](http://www.interspeech2020.org/uploadfile/pdf/Thu-3-1-3.pdf). 

<u>We do not compare to </u>:
who only report on the development set
([Williamson et al., 2016](https://dl.acm.org/doi/abs/10.1145/2988257.2988263?casa_token=ZB95uIA2YdMAAAAA%3AFlW-2gqwMNy2EWdqvWrJY39aJMlfLqzCr1HrfdbqZgewnGCPDbg3FkWWEWQ8MXyGz_5ZYREqg6k); [Haque et al., 2018](https://arxiv.org/pdf/1811.08592.pdf); [Al Hanai et al., 2018](https://groups.csail.mit.edu/sls/publications/2018/Alhanai_Interspeech-2018.pdf); [Dinkel et al. (2019)](https://arxiv.org/pdf/1904.05154.pdf); [Qureshi et al., 2020](https://ieeexplore.ieee.org/document/8910655)) 

###### Evaluation Metrics

For depression classification we follow [Dinkel et al. (2019)](https://arxiv.org/pdf/1904.05154.pdf) and report accuracy, macro-F1, precision, and recall. For emotion analysis, we follow [Cerisara, 2018](https://aclanthology.org/C18-1063.pdf)  and report macro-F1

##### Implementation Details 

==We implement our model with AllenNLP library== [Gardner et al., 2018](https://aclanthology.org/W18-2501.pdf) [see github](https://github.com/allenai/allennlp)

We use the original separation of train, validation, and test sets for both corpora. 
The model is trained for a maximum of 100 epochs with early stopping. 
For **STL** as well as for **MTL** scenario, we optimize on macro-F1 metric for depression classification. We use cross-entropy loss. The batch size is 4 for DailyDialog and 1 for DAIC (within the limit of GPU VRAM)

==We use the tokenizer from spaCy Library== and construct the word embeddings by default with a dimension of 128. The turn level has one hidden layer and 128 output neurons. We tune document **RNN** layers in {1, 2, 3} and hidden size in {128, 256, 512}. ==Model parameters are optimized using Adam== [Kingma and Ba, 2014](https://arxiv.org/pdf/1412.6980.pdf) with 1e − 3 learning rate. Dropout rate is set to 0.1 for both turn and document encoders

### Results

Results using our MTL hierarchical structure are shown in Table, which are compared to SOTA models (at the top). Our baseline model is a singletask naive hierarchical model which obtains similar results ($F_{1}$ 44) as the baseline model (NHN) in [Mallol-Ragolta et al. (2019)](https://opus.bibliothek.uni-augsburg.de/opus4/frontdoor/deliver/index/docId/65784/file/2036.pdf) ($F_{1}$ 45).

|                                                                                                                                       | $F_{1}$   | $Prec.$  | $Rec.$    | $Acc.$   |
| ------------------------------------------------------------------------------------------------------------------------------------- | --------- | -------- | --------- | -------- |
| BSL Majority vote                                                                                                                     | 41.3      | 35.1     | 50.0      | 70.2     |
| State-of-the-art                                                                                                                      |           |          |           |          |
| NHN5  [Mallol-Ragolta et al. (2019)](https://opus.bibliothek.uni-augsburg.de/opus4/frontdoor/deliver/index/docId/65784/file/2036.pdf) | 45        | -        | 50        | -        |
| HCAN6 [Mallol-Ragolta et al. (2019)](https://opus.bibliothek.uni-augsburg.de/opus4/frontdoor/deliver/index/docId/65784/file/2036.pdf) | 63        | -        | 66        | -        |
| HAN+L7 [Xezonaki et al. (2020)](http://www.interspeech2020.org/uploadfile/pdf/Thu-3-1-3.pdf).                                         | 70        | -        | 70        | -        |
| Ours:                                                                                                                                 |           |          |           |          |
| STL Depression                                                                                                                        | 43.9      | 44.5     | 47.5      | 63.8     |
| MTL +Emo                                                                                                                              | 55.5      | 56.2     | 61.6      | 70.2     |
| MTL +Top                                                                                                                              | 55.6      | 55.9     | 56.8      | 59.6     |
| MTL +Diag                                                                                                                             | 60.8      | 60.6     | 61.4      | 66.0     |
| MTL +Emo+Diag+Top                                                                                                                     | **70.6*** | **70.1** | **71.5*** | **74.5** |
**Depression detection results on DAIC**
	STL: single-task using DAIC only; MTL: multi-task using DAIC and adding classification for Emotion (+Emo), Topic (+Top), Dialog Act (+Diag) from DailyDialog. 
	- Significantly better than SOTA performance with p-value < 0.05.

==Depressed people tend to express specific emotions; it is thus natural to think that emotion is beneficial for the main task. These results indicate that both emotion and dialog structure help as they provide complementary information, paving the way for new research directions with more fine-grained modeling of dialog structure for tasks in conversational scenarios==


### Analysis

##### Performance on Auxiliary Tasks

To better understand our model, we look at the performance of emotion, dialog act, and topic auxiliary tasks. Directly comparing the results of our **MTL** approach (‘+Emo+Diag+Top’) with a **STL** architecture for each task, however, seems unfair. The optimized objective and structural complexity are different: the former is optimized on the depression detection task on two levels, while the latter is tuned on the target auxiliary task with either speech turn (emotion and dialog act) or full dialog (topic). Unsurprisingly, the results show that the **MTL** system underperforms the basic **STL** structure for dialog acts and topics, with at best 67.8 in
$F_{1}$ (**MTL**) vs. 68.8 (**STL**) for dialog acts, and 52.0 (**MTL**) vs. 52.4 (**STL**) for topic classification.

For emotion, on the other hand, our best **MTL** system obtains 40.0 in $F_{1}$ compared to 38.3 for the **STL** baseline, showing the mutual benefit of both tasks. Even though the score is lower than the **SOTA** for emotion classification (51.0 $F_{1}$ in Qin et al. (2021)) , we believe that refining our model for this task could lead to further improvements in depression detection. In addition, we observe that our **MTL** approach is particularly beneficial for negative and rare emotion classes, with *anger*, *disgust* and *sadness* gaining resp. 5%, 6% and 1% in $F_{1}$. Finally, we conduct a manual inspection of the types of utterances (mostly questions) from Ellie, and classify them into high-level dialog acts: *Backchannel*, Comment, Opening, Other, Question. (*Backchannel refers to phatic expressions such as yeah, hum mm. Here we use different dialog acts from those in DailyDialog*)

We find that around 13% of the utterances are emotion-related, for instance “things which make you mad / you feel guilty about, last time feel really happy”, etc., and that mentions of topics related to happiness or regret appear in almost all the interviews. Dialog act distribution is shown in Table. We release our annotation to the community for future studies.

| High-level_DA | #     | %   | Sub-cat. | #     | %   |
| ------------- | ----- | --- | -------- | ----- | --- |
| Question      | 7,907 | 53% | Emo      | 1,054 | 13% |
| Backchannel   | 3,231 | 22% | Non-emo  | 6,853 | 87% |
| Comment       | 3,074 | 20% | -        | -     | -   |
| Opening       | 611   | 4%  | -        | -     | -   |
| Other         | 171   | 1%  | -        | -     | -   |
	High-level dialog act distribution of Ellie in DAIC-WOZ. # and % represent the number and percentage of Ellie’s utterances, respectively
	
##### Effectiveness of Hierarchical Structure

To examine the effectiveness of hierarchical structure, we conduct ablation studies on the full multilearning setting (‘+Emo+Diag+Top’). For dialog RNN level, we use topic information; for turn level, we test either emotion or dialog act. The results are shown in Table. Unsurprisingly, both ablated models (‘+Emo+Top’ and ‘+Diag+Top’) underperform the full model, with $F_{1}$ scores decreasing ≈ 6% each. Without dialog act, all metrics drop, showing the importance of this information for dialog structure. Without emotion, recall drops dramatically while accuracy and precision increase, indicating that the model ‘+Diag+Top’ predicts more positive classes but fails in negative ones, which could result in too many false positives in real-life scenarios. On the other hand, when comparing hierarchical models (‘+Emo+Top’, ‘+Diag+Top’, ‘+Emo+Diag+Top’) with single-level models (‘+Emo’, ‘+Top’, ‘+Diag’), we see considerable improvements in all metrics, and this holds for all auxiliary tasks. We can thus confirm the advantage of hierarchical structure for model performance.

|                   | F1       | Prec.    | Rec.     | Acc.     |
| ----------------- | -------- | -------- | -------- | -------- |
| MTL +Emo+Diag+Top | **70.6** | 70.1     | **71.5** | 74.5     |
| MTL +Emo+Top      | 64.4     | 64.4     | 64.4     | 70.2     |
| MTL +Diag+Top     | 63.7     | **78.1** | 62.8     | **76.6** |
	 Ablation study on hierarchical structure


### Conclusion

In this paper, we demonstrate the correlation between depression and emotion and show the relevance of features related to dialog structures via shallow markers: dialog acts and topics. In the near future, we intend to investigate more refined modeling of dialog structures, possibly relying on discourse parsing ([Shi and Huang, 2019](https://dl.acm.org/doi/abs/10.1609/aaai.v33i01.33017007)). 
We would also like to explore depression severity classification as an extension to binary classification, possibly through a cascading structure: first detect depression and then classify the severity. We intend to refine our work and report on cross-validation splits of the data to test the stability of the model, an issue even more crucial when dealing with sparse data with possibly representativeness problem. A further step will be to investigate the generalization of our model to other mental health disorders.


### Auxiliary Tasks Class Distribution in DailyDialog Tables

![Pasted image 20240307160020.png](/img/user/Images/Pasted%20image%2020240307160020.png)



### #Useful_Features 



### #Source_code 

https://github.com/chuyuanli/MTL4Depr

