# Social Media Stance Detection under Fiancial Content
## Motivation
Since the difficulty of true labeling, <b>stance detection</b>, as one of the crucial sub-task of fake content detecion, has been receving more and more attention.

![GitHub Logo](https://dl.dropbox.com/s/xbguwgwyrv74cka/stat.jpg)
<p>Publications per year on stance detection as searched on Web of Science. The following keywords were used for search: “stance detection”, “stance prediction” and “stance classification"</p>

## Related Work and Publications (More previous articles to be added)
1. Stance Detection on Social Media: State of the Art and Trends (2021): https://arxiv.org/pdf/2006.03644
2. Stance detection using improved whale optimization algorithm (2021): https://link.springer.com/article/10.1007/s40747-021-00294-0#Sec9
3. Adversarial Learning for Zero-Shot Stance Detection on Social Media (2021): https://paperswithcode.com/paper/adversarial-learning-for-zero-shot-stance
4. Target-Aware Data Augmentation for Stance Detection (2021): https://aclanthology.org/2021.naacl-main.148/
5. Knowledge Enhanced Masked Language Model for Stance Detection (2021): https://paperswithcode.com/paper/knowledge-enhanced-masked-language-model-for
6. Stance Detection with BERT Embeddings for Credibility Analysis of Information on Social Media (2021): https://arxiv.org/abs/2105.10272
7. Zero-Shot Stance Detection: ADataset and Model using Generalized Topic Representations (2020): https://arxiv.org/abs/2010.03640
8. Recognising Agreement and Disagreement between Stances with Reason Comparing Networks (2019): https://aclanthology.org/P19-1460/ 
9. Can Siamese Networks help in stance detection? (2019): <a href="/src/SiamNet.pdf" class="image fit">Link to pdf (acquired from UDEL library and cannot be distributed)</a>
10. Universal Language Model Fine-tuning for Text Classification (2018): https://arxiv.org/abs/1801.06146
11. Stance Classification of Context-Dependent Claims (2017): https://aclanthology.org/E17-1024/
12. MITREatSemEval-2016 Task 6: Transfer Learning for Stance Detection (2016): https://arxiv.org/abs/1606.03784

## Data Set
✅ Repository for the ACL 2020 paper:
Will-They-Won't-They: A Very Large Dataset for Stance Detection on Twitter <br>

Will-They-Won't-They (WT-WT) is a large dataset of English tweets targeted at stance detection for the rumor verification task. The dataset is constructed based on tweets that discuss five recent merger and acquisition (M&A) operations of US companies, mainly from the healthcare sector.

All the annotations are carried out by domain experts; therefore, the dataset constitutes a high-quality and reliable benchmark for future research in stance detection.

Paper Link: https://arxiv.org/abs/2005.00388 <br>

### Test Train Split:

```
from sklearn.model_selection import train_test_split
import pandas as pd

data_org = pd.read_csv('data/full_data.csv')
train, test = train_test_split(data_org, test_size=0.3, stratify=data_org['merger'])
train.to_csv('data/train_data.csv', index = False)
test.to_csv('data/test_data.csv', index = False)
```

### Stances:

1. Support: the tweet is stating that the two companies will merge.
```
[CI_ESRX] Cigna to acquire Express Scripts for $52B in health care shakeup via usatoday
```

2. Refute: the tweet is voicing doubts that the two companies will merge.
```
[AET_HUM] Federal judge rejects Aetna’s bid to buy Louisville-based Humana for $34 billion
```

3. Comment: the tweet is commenting on merger, neither directly supporting, nor refuting it.
```
[CI_ESRX] Cigna-Express Scripts deal unlikely to benefit consumers
```

4. Unrelated: the tweet is unrelated to merger.
```
[CVS_AET] Aetna Announces Accountable Care Agreement with Weill Cornell Physicians
```

### Considered M&A operations


| Operation | Buyer       | Target            | Industry
| ---       | ---         | ---               | ---
| CVS_AET   | CVS Health  | Aetna             | Healthcare
| CI_ESRX   | Cigna       | Express Scripts   | Healthcare
| ANTM_CI   | Anthem      | Cigna             | Healthcare
| AET_HUM   | Aetna       | Humana            | Healthcare
| DIS_FOXA  | Disney      | 21st Century Fox  | Entertainment

<br>

## Addtional datasets:
1. <b>Self GoogleNews collection of 319 articles. (under review)</b>
2. <b>STANDER:</b>

    STANDER is a large dataset of news articles in English from high-reputation sources which discuss four recent mergers and acquisitions (M&A) operations between major healthcare companies in the US. The news articles are annotated by experts and labeled for stance detection and fine-grained evidence retrieval.
    
    STANDER contains ✅ <b> the same targets as in the Twitter stance detection WT–WT corpus WT–WT corpus </b> (Conforti et al., 2020). The union of both corpora thus provides a great opportunity for studying the interplay between authoritative and user-generated signals.
    
    Paper Link: https://aclanthology.org/2020.findings-emnlp.365/
    
    GitHub Link: https://github.com/cambridge-wtwt/emnlp2020-stander-news
    
    ✅ Note: Not avaiable yet but I already sent the email to request the data and hopefully, they will get back to me soon.
    

## Useful/Baseline Methodologies:

Overall, three major component for stance detection problems: Language Model, Classifier, and Evaluation
### Language model (❌ haven't tried yet)
#### <a name="introduction"></a> BERTweet: A pre-trained language model for English Tweets 

BERTweet is the first public large-scale language model pre-trained for English Tweets. BERTweet is trained based on the [RoBERTa](https://github.com/pytorch/fairseq/blob/master/examples/roberta/README.md)  pre-training procedure. The corpus used to pre-train BERTweet consists of 850M English Tweets (16B word tokens ~ 80GB), containing 845M Tweets streamed from 01/2012 to 08/2019 and 5M Tweets related to the **COVID-19** pandemic. The general architecture and experimental results of BERTweet can be found in [paper](https://aclanthology.org/2020.emnlp-demos.2/):
### ULMFit (⭕ already tried)

#### Architecture:
![GitHub Logo](https://dl.dropbox.com/s/orvx5r4tmsalebc/ulmfit_arch.png)
#### 3-stage fine-tuning methodology
The classification task is done in a 3-stage process:

1. <b>General-domain LM pretraining:</b> ULMFit has a pretrained model generated using an AWD-LSTM (as per Merity et al., 2017)) to develop a language model called Wikitext-103 and was trained of 28,595 preprocessed Wikipedia articles, totalling to 103 million words.
2. <b>Target task LM fine-tuning:</b> Since the data for the target will likely come from a different distribution, ULMFit allows us to use the pre-trained language model anf fine-tune it (using the above techniques) to adapt to the idiosyncrasies of the target data.
3. <b>Target task classifier fine-tuning:</b> Once we save the updated weights from the language model fine-tuning step, we can fine-tune the classifier with gradual unfreezing and the other techniques described above to perform task-specific class prediction.

<b>Note:</b> for this stance classification task, only steps 2 and 3 are performed, and utilize the pretrained language model (step 1) from the fastai database.

Method Offical Link: https://humboldt-wi.github.io/blog/research/information_systems_1819/group4_ulmfit/

My Colab workpage (with comments): https://drive.google.com/file/d/1_UtMB7Nyv8zxYO9zPwxyQ__8Ok0iPDpD/view?usp=sharing


### SiamNet (❌ haven't tried yet)

