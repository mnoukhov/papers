---
attachments: [Clipboard_2020-10-27-21-54-51.png, Clipboard_2020-10-27-21-55-36.png]
title: Deep contextualized word representations
created: '2020-10-28T01:50:30.179Z'
modified: '2020-10-28T02:17:55.005Z'
---

# Deep contextualized word representations

# overview

- ELMo: BiLSTM pretrained word embeddings
- large SOTA on six benchmarks using their baseline methods
- deep representations are better than just top layer of LSTM

## ELMo
![](@attachment/Clipboard_2020-10-27-21-54-51.png)

ELMo
- context-independent token embedding $t_k$ from CNN over chars or token embedding
- $L$ layers of BiLSTM over all tokens $k$ to create hidden layers $h_{k,j}^{LM}$
- task-specific embedding, weighted sum $ELMo_k^{task} = \gamma^{task} \sum_{j=0}^L s_j^{task} h_{k,j}^{LM}$ 
- pretrain on two-way language modelling $\sum_k p(t_k|t_1, t_2,\ldots t_{k-1}) + p(t_k | t_{k+1}, t_{k+2},\ldots, t_N)$, softmax over top $h_k$ to predict token.

Supervised Learning
- freeze ELMo, use embed input $x_k \to [x_k;ELMO_k^{task}]$ and feed into RNN
- for SNLI, SQuAD also add to RNN output $h_k \to [h_k;ELMO_k^{task}]$ 
- 0.5 dropout
- $\lambda ||w||^2$ regularization

Architecture
- 2 BiLSTM layers
- 4096 units, 512 dim, residual layer
- $t_k$ using 2048 character n-gram conv filter, 2-layer highway network, linear layer 512 dim

## results

![](@attachment/Clipboard_2020-10-27-21-55-36.png)

SOTA on a bunch of NLP tasks
- using ELMo on baseline models gets SOTA
- substituting CoVe for ELMo sees improvements too

design analysis
- weighted output of layers beats average or just using top layer
- ELMo at input + RNN output can be useful
- ELMO clearly learns contextual info 
  - nearly SOTA on word sense disambiguation just using first layer
  - nearly SOTA for POS tagging on PTB just using second layer
- models with ELMo are much better in low data regime (pretraining!)

## citation

```
@inproceedings{peters2018elmo,
  author={Peters, Matthew E. and  Neumann, Mark and Iyyer, Mohit and Gardner, Matt and Clark, Christopher and Lee, Kenton and Zettlemoyer, Luke},
  title={Deep contextualized word representations},
  booktitle={Proc. of NAACL},
  year={2018}
}
```
