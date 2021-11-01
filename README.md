# Social Media Stance Detection under Fiancial Content
## Motivation
Since the difficulty of true labeling, <b>stance detection</b>, as one of the crucial sub-task of fake content detecion, has been receving more and more attention.

![GitHub Logo](https://dl.dropbox.com/s/xbguwgwyrv74cka/stat.jpg)
<p>Publications per year on stance detection as searched on Web of Science. The following keywords were used for search: “stance detection”, “stance prediction” and “stance classification"</p>

## Dataset (STANDAR)
STANDER ts a large dataset of news articles in English from high-reputation sources which discuss four recent mergers and acquisitions (M&A) operations between major healthcare companies in the US. The news articles are annotated by experts and labeled for stance detection and fine-grained evidence retrieval.

Paper Link: https://aclanthology.org/2020.findings-emnlp.365/
### Stances:

Four Categories: 🙂 Support ☹️ Refute 😑 Comment and 😕 Unrelated

Example 💬
```
Stance: support
Target: AET_HUM
Title: Aetna to Acquire Humana for $37 Billion
Body: Aetna (NYSE: AET) and Humana Inc. (NYSE: HUM) today announced that they have entered into a definitive agreement under swhich Aetna will acquiure all outstanding shares of Humana for a combination of cash and stock [...]
```

### Mergers:
| Operation | Buyer       | Target            | Industry    | Outcome
| ---       | ---         | ---               | ---         | ---
| CVS_AET   | CVS Health  | Aetna             | Healthcare  | 💚 succeeded 
| CI_ESRX   | Cigna       | Express Scripts   | Healthcare  | 💚 succeeded 
| ANTM_CI   | Anthem      | Cigna             | Healthcare  | 💔 rejected 
| AET_HUM   | Aetna       | Humana            | Healthcare  | 💔 rejected 

### EDA:
Article Count by Merger    |  Article Count by Merger and Stance 
:-------------------------:|:-------------------------:
![pic1](/src/pic1.png)  |  ![pic2](/src/pic2.png) 
```
The maximun number of token for BERT is 512, but some articles are longer.
Text was truncated by 512 because 👇 the distribution of evidence location
```
<img src="/src/Capture1.PNG" width="50%" height="30%">

### BERT-base-cased + CNN Result:
|Name      |Accuracy|F1-Avg|F1-Weighted|comment_f1-score|refute_f1-score|support_f1-score|
|----------|--------|------|-----------|----------------|---------------|----------------|
|AET_HUM_NT|0.606   |0.565 |0.602      |0.355           |0.692          |0.646           |
|ANTM_CI_NT|0.645   |0.605 |0.639      |0.416           |0.709          |0.688           |
|CVS_AET_NT|0.574   |0.533 |0.525      |0.297           |0.623          |0.679           |
|CI_ESRX_NT|0.628   |0.596 |0.638      |0.537           |0.551          |0.699           |
|AET_HUM   |0.577   |0.553 |0.579      |0.376           |0.696          |0.587           |
|ANTM_CI   |0.650   |0.608 |0.645      |0.422           |0.725          |0.678           |
|CVS_AET   |0.591   |0.559 |0.560      |0.378           |0.611          |0.689           |
|CI_ESRX   |0.677   |0.615 |0.678      |0.571           |0.508          |0.767           |


### Biases in the Dataset
1. the result shown above indicates that the CNN model puts less weights on "Target", which is not explanatory. 
2. Unbalanced stance distribution. Thus, predictions on Unrelated/Comment are less accurate.


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
### ✅ ACL proceeding: tWT–WT: ADataset to Assert the Role of Target Entities for Detecting Stance of Tweets

link: https://www.aclweb.org/anthology/2021.naacl-main.303.pdf


## Addtional datasets:
### Will-They-Won't-They: A Very Large Dataset for Stance Detection on Twitter
Will-They-Won't-They (WT-WT) is a large dataset of English tweets targeted at stance detection for the rumor verification task. The dataset is constructed based on tweets that discuss five recent merger and acquisition (M&A) operations of US companies, mainly from the healthcare sector.

Paper Link: https://arxiv.org/abs/2005.00388 
    


---
# Below works are done for Tweets dataset

## Useful/Baseline Methodologies:

Overall, three major component for stance detection problems: Language Model, Classifier, and Evaluation
### Language model (⭕)
#### <a name="introduction"></a> BERTweet: A pre-trained language model for English Tweets 

Colab: https://drive.google.com/file/d/1EM2pg0ampWIYBRMOVf9km3EdoqTDk4jj/view?usp=sharing

BERTweet is the first public large-scale language model pre-trained for English Tweets. BERTweet is trained based on the [RoBERTa](https://github.com/pytorch/fairseq/blob/master/examples/roberta/README.md)  pre-training procedure. The corpus used to pre-train BERTweet consists of 850M English Tweets (16B word tokens ~ 80GB), containing 845M Tweets streamed from 01/2012 to 08/2019 and 5M Tweets related to the **COVID-19** pandemic. The general architecture and experimental results of BERTweet can be found in [paper](https://aclanthology.org/2020.emnlp-demos.2/):
### ULMFit (⭕ already tried)

#### Architecture:
![GitHub Logo](https://dl.dropbox.com/s/orvx5r4tmsalebc/ulmfit_arch.png)
#### 3-stage fine-tuning methodology
The classification task is done in a 3-stage process:

1. <b>General-domain LM pretraining:</b> ULMFit has a pretrained model generated using an AWD-LSTM (as per Merity et al., 2017)) to develop a language model called Wikitext-103 and was trained of 28,595 preprocessed Wikipedia articles, totalling to 103 million words.
2. <b>Target task LM fine-tuning:</b> Since the data for the target will likely come from a different distribution, ULMFit allows us to use the pre-trained language model anf fine-tune it (using the above techniques) to adapt to the idiosyncrasies of the target data.

        -use full training set for fine-tuning

4. <b>Target task classifier fine-tuning:</b> Once we save the updated weights from the language model fine-tuning step, we can fine-tune the classifier with gradual unfreezing and the other techniques described above to perform task-specific class prediction.

        -use specific topic sub-set for training classifier
        -test using the seperate test set

<b>Note:</b> for this stance classification task, only steps 2 and 3 are performed, and utilize the pretrained language model (step 1) from the fastai database.

Method Offical Link: https://humboldt-wi.github.io/blog/research/information_systems_1819/group4_ulmfit/

My Colab workpage (with comments): https://drive.google.com/file/d/1_UtMB7Nyv8zxYO9zPwxyQ__8Ok0iPDpD/view?usp=sharing

#### Result Report (Accuracy):

| Operation | Overall Accuracy       | F-1   (comment, refute, support, unrelated)      
| ---       | ---         | ---               
| CVS_AET   | 0.7943791224548322  | [0.800722, 0.627451, 0.787743, 0.809131]             
| CI_ESRX   | 0.7401055408970977       | [0.720539, 0.642336, 0.77533 , 0.767372]   
| ANTM_CI   | 0.833383640205252      | [0.737534, 0.765203, 0.711755, 0.943925]             
| AET_HUM   | 0.8003376952300548       | [0.750296, 0.714065, 0.750469, 0.889722]           
| DIS_FOXA  | 0.8701227331013006      | [0.873879, 0.659794, 0.758621, 0.892416]  

### SiamNet (⭕)

