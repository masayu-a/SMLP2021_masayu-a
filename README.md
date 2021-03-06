# SMLP2021_masayu-a

## Description of this repository.

This repository includes the pre-report files for SMLP2021: https://vasishth.github.io/smlp2021/
I am an participant of the `Advenced methods in frequentist statistics with Julia'.


## Data
### 07rt-OW-unmasked2.txt
The original file is from https://github.com/masayu-a/BCCWJ-SPR2/
The data includes reading time data by self paced reading.
The text data is whitepaper by the ministry of education, culture, sports, and technology, Japan.

> 文部科学省白書 文部科学省 (2005) （独）国立印刷局

#### how the data collect?
The data is collected using [ibexfarm](https://spellout.net/ibexfarm).
The subject participants were recruited using [Yahoo! Crowdsourcing](https://crowdsourcing.yahoo.co.jp/).

The settings are in https://github.com/masayu-a/ibexfarm_OW6X_00000

#### format
- **BCCWJ_Sample_ID**	    Sample ID in BCCWJ
- **BCCWJ_start**		    Offset in BCCWJ
- **SPR_sentence_ID**	    Sentence ID in ibexfarm
- **SPR_bunsetsu_ID**	    Phrase ID in ibexfarm
- **SPR_surface**		    Surface form
- **DepPara_depnum**	    Number of the dependent
- **SPR_word_length**	    Character number of the surface form
- **SPR_reading_time**	    Reading time (msec)
- **SPR_subj_ID**		    ID of subject participants in self paced reading experiment
- **WFR_subj_rate**		    vocab test results of the subject participant in word familiarity rate

The composite primary key is [SPR_sentence_ID, SPR_bunsetsu_ID, SPR_subj_ID].

#### what is word familiarity rate and vocab test results?

We performed word familiarity rate estimation experiments.
https://github.com/masayu-a/WLSP-familiarity

> Masayuki Asahara (2019) Word Familiarity Rate Estimation Using a Bayesian Linear Mixed Model, Proceedings of the First Workshop on Aggregating and Analysing Crowdsourced Annotations for NLP, pages 6-14. https://www.aclweb.org/anthology/D19-5902.pdf

We used the random effect of the subject participants as the result of vocab test.

## Analysis
### 07rt-OW.Rmd

We modeled the reading time and log time of the data.

```{r model_all}
model_all = lmer(SPR_reading_time~SPR_sentence_ID+SPR_bunsetsu_ID+SPR_word_length+DepPara_depnum+WFR_subj_rate+(1|SPR_subj_ID_factor),data=d,REML=FALSE)
summary(model_all)
stargazer(model_all)
```

- SPR_sentence_ID is the sentence presentation order
- SPR_bunsetsu_ID is the phrase presentation order in a sentence
- SPR_word_length is the character number of the surface form
- DepPara_depnum is the attachment number of the dependent in the dependency structures. Note that, since Japanese is strictly head final language, the dependents always precede to the head phrase.
- WFR_subj_rate is the result of vocab test.
- (1|SPR_subj_ID_factor) is the random effect of the subject participants

## License

CC BY 4.0
