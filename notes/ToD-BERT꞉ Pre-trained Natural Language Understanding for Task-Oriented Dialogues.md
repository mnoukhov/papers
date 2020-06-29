---
title: 'ToD-BERT: Pre-trained Natural Language Understanding for Task-Oriented Dialogues'
created: '2020-06-22T15:47:44.145Z'
modified: '2020-06-22T16:13:56.762Z'
---

# ToD-BERT: Pre-trained Natural Language Understanding for Task-Oriented Dialogues

## overview
- pretraining strategy for dialogue systems, MLM and Response Selection
- 9 combined dialogue datasets for pretraining
- final model is better on downstream tasks useful for dialogue

## datasets
- MetaLWOZ
- Schema
- Taskmaster
- MWOZ
- MSR E2E
- SMD
- WOZ, CamRest676

## ToD-BERT
initialize model with BERT and pretrain

MLM
- concatenate system and user dialogues
- separate each with `[SYS]` and `[USR]` tokens
- predict `[MASK]` tokens as usual

Response Selection Loss
- Separately encode context ${S_1, U_1, \ldots U_t}$ and response $S_{t+1}$ using `[CLS]` embedding
- Use other contexts, responses in batch as negative samples
- Predict whether response follows context

## experiments

### metrics

intent classification
- predict one of $M$ intents given a sentence

dialogue state tracking
- predict `(domain, slot)` pair for each dialogue turn

dialogue act prediction
- predict which of $N$ acts need to be taken after each dialogue turn
- each turn can be many acts

response selection
- rank possible response candidates 
- use random responses as negative samples
- predict ranking of true response using cosine similarity given encodings


### datasets
- OOS
- DSTC2
- GSIM
- MWOZ (test set)

### results
consistently outperforms BERT 
- on pretty much all tasks
- on few-shot and low-data regimes and full data

outperforms GPT2, DialoGPT, other baselines
- on all tasks
- on full data

MLM+RSL training is sometimes not necessary
- MLM alone does better in simpler tasks

Turning embeddings into clusters favours ToD-BERT
- k-means to get clusters on tSNE
- ToD clusters have higher MI than regular BERT, better representations

## citation

```
@misc{wu2020todbert,
    title={ToD-BERT: Pre-trained Natural Language Understanding for Task-Oriented Dialogues},
    author={Chien-Sheng Wu and Steven Hoi and Richard Socher and Caiming Xiong},
    year={2020},
}
```
