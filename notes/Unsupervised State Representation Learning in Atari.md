---
attachments: [Clipboard_2020-11-09-21-58-11.png]
title: Unsupervised State Representation Learning in Atari
created: '2020-11-10T02:54:13.143Z'
modified: '2020-11-10T05:09:29.977Z'
---

# Unsupervised State Representation Learning in Atari

## overview

- maximize MI across features for RL
- Atari benchmark for state representation
- extensive comparison of current SSL techniques on Atari

## Spatiotemporal Deep Infomax (ST-DIM)

DIM between consequetive frames 
- sum of patch-level MI
- global-local: MI between $x_{t+1}$ local spatial feature at $m,n$ and $x_t$ global feature
- local-local: MI between $x_{t+1}, x_t$ at local spatial feature $m,n$

Training 
- [infoNCE](@note/Representation Learning with Contrastive Predictive Coding)
- calculate MI with bilinear model $f(x,y) = \phi(x) W^T \phi(y)$ 

![](@attachment/Clipboard_2020-11-09-21-58-11.png)

## Atari Annotated RAM Interface (AtariARI)

decompile RAM and annotate state variables
- agent loc: x,y of agent
- small object loc: x,y of balls, missiles etc
- other loc: x,y of other sprites, enemies
- score / clock / lives / display
- misc: game-specific e,g, pin in Breakout, room num in Montezuma

## experiment setup

baselines
- random CNN
- VAE on raw obs
- next-step pixel prediction
- CPC
- supervised encoder, with linear probe on labels (upper bound on perf)

training and eval
- data from random agent or 50M trained PPO with epsilon-greedy (0.2)
- train on 35k frames, 5k eval, 10k test
- predict each of 256 bytes of RAM

ST-DIM generally outperforms other methods
- contrastive methods (CPC) do better than generative
- random CNN isn't too bad of a baseline
- sizeable gap $0.68 \to 0.82$ to supervised performance

Ablations
- InfoNCE is better than NCE (JSD)
- spatial is important, better than just temporal
- ST-DIM (and contrastive) do better on small objects
- contrastive captures high entropy (e.g. visible clock) 
- generative captures low entropy but impactful in pixel-space (e.g. y coordinate in pong)

## citation

```
@inproceedings{anand2019-representation63,
    author = {Ankesh Anand and Evan Racah and Sherjil Ozair and Yoshua Bengio and Marc-Alexandre Cote and R. Devon Hjelm},
    title = {Unsupervised State Representation Learning in Atari},
    year = {2019},
    booktitle = {Advances in Neural Information Processing Systems},
    pages = {8769-8782}
}
```
