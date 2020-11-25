---
attachments: [Clipboard_2020-11-13-22-09-24.png]
title: 'SenseBERT: Driving Some Sense into BERT'
created: '2020-11-14T02:53:57.911Z'
modified: '2020-11-15T04:51:27.155Z'
---

# SenseBERT: Driving Some Sense into BERT

## overview

- WordNet supersense prediction as extra BERT SSL task
- better lexical improved results on SemEval, WSD
- SOTA on "Word in Context"

## SenseBERT

![](@attachment/Clipboard_2020-11-13-22-09-24.png)

45 WordNet supersenses
- 26 nouns
- 15 verbs
- 3 adjectives
- 1 adverb

embed into supersense
- embed input $Sx^{(i)}$ to 45d wordsense
- add wordsense to embedding and pos $v_{input} = (W + SM)x + p$
- softmax over embedded BERT output $S^T v_{output}^{(i)}$   

learn supersense
- WordNet lemmatize $w$, get synsets, get all supersenses $A(w)$ associated with synsets
- soft cross-entropy on *any* of superset $\mathcal{L}^{allowed} = - \log \sum_{s \in A(w)} p(s | context)$
- regularization to be uniform across $A(w)$, $\mathcal{L}^{reg} = - \frac{1}{|A(w)|} \sum_{s \in A(w)} \log p(s | context)$

dealing with subwords
1. *60k vocab*: adds 30k words to vocab using wikipedia frequency
    if you still get subword, don't train supersense prediction
2. *average embedding*: combine all tokens of OOV and mask together
    train WordNet supersense using average of output embeddings of all masked subwords

model
- BERT base
- choose words with one supersense more frequently (50% of possible up to 40% of 0.15 masking budget)

## experiments

pre-trained SenseBERT mapping of supersense prediction $S$ shows visible supersense clusters

word supersense disambiguation (SemEval-SS)
- SemCor train, SenseEval/SemEval test 
- predict WordNet supersense

*average* and *60k* are both good for rare words on SemEval-SS
- modest 1% improvement over baseline

SenseBERT shows better performance across linear probe/finetune and base/large
- linear probing SenseBERT $79.5\%$ beats BERT $67.3\%$
- finetune SenseBERT $83.7$ beats BERT $81.1$ 

SenseBERT gets SOTA on Word-in-Context 
- predict whether two instances of a word have the same meaning (given context)
- defined over supersenses
- beats RoBERTa and KnowBERT+tricks

SenseBERT is nearly identical to BERT on GLUE
- seemingly no loss in other performance

## notes

sense-word mismatch
- since predictions are parallel it is possible to predict one sense and a word that doesn't match the sense
- wouldn't it be more logical to have a system that predicts $p(sense|context)$ and a second that predicts $p(word|sense, context)$

## citation

```
@inproceedings{levine2020-sensebert36,
    author = {Yoav Levine and Barak Lenz and Or Dagan and Dan Padnos and Or Sharir and Shai Shalev-Shwartz and Amnon Shashua and Yoav Shoham},
    title = {SenseBERT: Driving Some Sense into BERT},
    year = {2020},
    booktitle = {Proceedings of the 58th Annual Meeting of the Association for Computational Linguistics},
    pages = {4656-4667},
    publisher = {Association for Computational Linguistics}
}
```
