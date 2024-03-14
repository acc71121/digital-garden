---
{"dg-publish":true,"permalink":"/datasets/sncdl/","tags":["gardenEntry"]}
---

### Deep learning for suicide and depression identification with unsupervised label correction

The dataset contains 1,895 total posts. We utilize two fields from the scraped data: the original text of the post as our inputs, and the subreddit it belongs to as labels. Posts from r/SuicideWatch are labeled as suicidal, and posts from r/Depression are labeled as depressed. 
We make this dataset and the web-scraping script available in our code. Furthermore, we use the Reddit Suicide C-SSRS datasetvto verify our label correction methodology. The C-SSRS dataset contains 500 Reddit posts from the subreddit r/depression. These posts are labeled by psychologists according to the Columbia Suicide Severity Rating Scale, which assigns progressive labels according to severity of depression. We use this dataset to validate our label correction method since the labels are clinically verified and from the same domain of Reddit. To further validate the label correction method, we use the IMDB large movie dataset, a commonly used NLP benchmark dataset. 

The dataset is a binary classification task which contains 50,000 polar movie reviews. We use a random subset of samples for evaluation. For comparison of our method against other related tasks and methods, we build a dataset for binary classification of clinically healthy text vs suicidal text. We utilize the two subreddits r/CasualConversation and r/SuicideWatch. r/CasualConversation is a subreddit of general conversation, and has generally been used by other methods as data for a clinically healthy class.

https://github.com/ayaanzhaque/SDCNL

#dataset