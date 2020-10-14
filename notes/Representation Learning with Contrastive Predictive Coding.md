---
attachments: [Clipboard_2020-09-23-12-25-08.png, Clipboard_2020-09-23-17-40-53.png, Clipboard_2020-09-23-19-54-54.png]
tags: [self-supervised]
title: Representation Learning with Contrastive Predictive Coding
created: '2020-09-22T19:35:03.261Z'
modified: '2020-09-24T23:06:09.563Z'
---

# Representation Learning with Contrastive Predictive Coding

## overview

- predict future latent space from current using autoregressive model
- show power of CPC on images, speech, NLP, RL

## CPC

motivation
- model important conditional features
- MSE and cross entropy don't capture these features
- big generative models capture too many useless features
- idea: maximize MI between context and input in latent representation

CPC
- embed input $z_t = g_{enc}(x_t)$
- embed contest with auto-regressive $c_t = g_{ar}(z_{\leq t})$
- use density ratio $f_k(x_{t+k},c_t) = \exp(z^T_{t+k}W_kc_t)$

InfoNCE
- $X$ has 1 sample from $p(x_{t+k}|c_t)$ and $N-1$ from $p(x_{t+k}|c_t)$
![](@attachment/Clipboard_2020-09-23-12-25-08.png)

## experiments

### librispeech

![](@attachment/Clipboard_2020-09-23-19-54-54.png)

task
- aligned phone(me) sequences for 100 hour subset
- divide into 10ms steps, predict 12 steps ahead
- encoder is CNN directly on waveform
- AR is GRU with input being each time step

results
- close to supervised on phone, very close on speaker classification
- maximum context size is important, model uses approx 1 sec

### imagenet

![](@attachment/Clipboard_2020-09-23-17-40-53.png)

SSL vision on ImageNet
- get 7x7 grid of 64x64 crops, encode, and predict future rows(?)
- encoder is resnet101v2, no batch norm
- AR is PixelCNN
- linear finetune on top of features

results
- huge 9% top-1 improvement
- pretty apples-to-apples comparison against other SSL tasks using ResNet101v2

CPCv2 has certain improvements
- patches in all directions
- lots of data augmentation (e.g. spatial and color jitter)
- 8% top-1 improvement
- data-efficiency improvement from CPC

### NLP

training
- train on BookCorpus
- test-time map word2vec to learned embedding to capture unseen words

test with 10-fold cross-validation
- movie review sentiment (MR)
- customer product reviews (CR)
- subjectivity/objectivity
- opinion polarity (MPQA)
- train/test split on question-type classification (TREC)

model
- encode a sentence with 1D conv + ReLU + mean-pool to 2400-dim
- AR is GRU predicting embeddings of next 3 sentences (kinda like skip-thought)

### RL

setup 
- 5 DM lab envs
- add CPC as auxiliary loss to A2C agent
- predict agent's latent 30 steps in the future

results 
- strong improvement on memory-based games
- no improvement in laser-tag (reaction-based?)

## discussion

MINE gave identical performance when the task was non-trivial but failed if it was easy


## citation

```
@article{oord2018-representation21,
    author = {Aaron van den Oord, Yazhe Li and Oriol Vinyals},
    title = {Representation Learning with Contrastive Predictive Coding},
    year = {2018},
    booktitle = {arXiv preprint arXiv:1807.03748}
}
```
