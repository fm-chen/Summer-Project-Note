# Social Media Stance Detection under Fiancial Content
## Motivation
TBD

## Related Work and Publications
TBD


## Data Set
<b>Repository for the ACL 2020 paper:
Will-They-Won't-They: A Very Large Dataset for Stance Detection on Twitter </b><br>

Will-They-Won't-They (WT-WT) is a large dataset of English tweets targeted at stance detection for the rumor verification task. The dataset is constructed based on tweets that discuss five recent merger and acquisition (M&A) operations of US companies, mainly from the healthcare sector.

All the annotations are carried out by domain experts; therefore, the dataset constitutes a high-quality and reliable benchmark for future research in stance detection.

Paper Link: https://arxiv.org/abs/2005.00388 <br>
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

## Methodology

Overall, three major component for stance detection problems: Language Model, Classifier, and Evaluation

### ULMFit
Offical Link: https://humboldt-wi.github.io/blog/research/information_systems_1819/group4_ulmfit/

