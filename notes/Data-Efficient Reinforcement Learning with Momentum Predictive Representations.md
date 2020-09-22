---
attachments: [Clipboard_2020-09-20-16-35-58.png, Clipboard_2020-09-20-16-38-06.png]
tags: [drl, self-supervised]
title: Data-Efficient Reinforcement Learning with Momentum Predictive Representations
created: '2020-09-20T19:55:54.459Z'
modified: '2020-09-21T20:37:43.892Z'
---

# Data-Efficient Reinforcement Learning with Momentum Predictive Representations

## overview

- Momentun Predictive Rep (MPR) predict future latent representation 
- forces data augmentation representations to be consistent
- better 100k step performance on Atari

## MPR

![](@attachment/Clipboard_2020-09-20-16-38-06.png)

Auxiliary loss on Data-efficient Rainbow which combines
- distributional RL
- dueling DQN
- double DQN

Predict state $K=5$ steps into the future
- using online encoding $\hat z_t$ = $f_o(s_t)$
- predict exponential moving average (momentum) encoding $\tilde z_t = f_m(s_t)$ for $s_{t+1:t+K}$
- iteratively apply transition model $h(\hat z_{t+k}, a_{t+k})$ to get next step

Project encodings into smaller space to compute losses
- $\tilde y_t = g_m(\tilde z_t)$ for projection
- extra prediction head for $\hat y_t = q(g_o(\hat z_t))$
- loss is cosine similarity (proportional to normalized $L_2$ of [BYOL](@note/Bootstrap Your Own Latent: A New Approach to Self-Supervised Learning))

![](@attachment/Clipboard_2020-09-20-16-35-58.png)

Data augmentation
- small random shifts and color jitter
- normalize activations of encoder and transition model if augmenting
- use dropout if not augmenting

## experiments

100k steps Atari (400k frames)
- SOTA (slight improvement over prev w/o data augment)
- 0.444 human-normalized w/ data augment

Ablations
- momentum encoder outperforms online encoder
- MPR w/o temporal prediction still better than prev SOTA (better use of data augment?)
- $K=5$ steps outperforms $1$ and $0$ steps
- better than contrastive loss in a pseudo-apples-to-apples approach

## discussion

good transition model performs worse




## citation

```

```
