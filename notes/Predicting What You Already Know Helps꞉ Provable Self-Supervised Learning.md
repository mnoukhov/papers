---
tags: [self-supervised, theory]
title: 'Predicting What You Already Know Helps: Provable Self-Supervised Learning'
created: '2020-09-14T19:30:40.583Z'
modified: '2020-09-15T22:13:44.320Z'
---

# Predicting What You Already Know Helps: Provable Self-Supervised Learning

## overview

- propose conditional independence to expain how solving a pretext task is useful for downstream task
- theoretically and empirically demonstrate reduced downstream sample complexity under assumption

## setup

notation
- $X_1$ input varible
- $X_2$ pretext task target
- $Y$ downstream task target
- $\psi$ pretext representation

error
- approximation error: error learning a linear layer on top of pretext representation 
- estimation error: diff between approximation error and linear finetune error for $n_2$ samples (sample complexity)

data assumption for $Y = f^*(X_1) + N$ where $f* = \mathbb{E}[Y|X_1]$
- $N$ is subgaussian (has a Gaussian tail)
- non-degeneracy in $X_1,X_2$

if we know it is subgaussian and independent we can find the number of samples necessary to find the true mean of the distribution within some error rate $\delta$ (which we can use to find sample complexity)

## proof
### recipe

find closed form expression for $\psi*$
use CI to argue that $e_{apx}(\psi*)$ is small

### warm-up: jointly gaussian variables

1. assume $X_1,X_2,X_3$ are jointly gaussian
2. assume $X_1$ CI $X_2 | Y$

claim (3.1)
- the closed form solution $\psi^*$ is $\Sigma_{X_2 X_1}\Sigma^{-1}_{X_1 X_1}$
- the target $f^*$ is $\Sigma_{Y X_1}\Sigma^{-1}_{X_1 X_1}$

theorem (3.3) 
- trace is the unaccounted variance in Y given X_1

generalize to latent variable
- condition on $Z \in \mathbb{R}^m$ as well as $Y$
- replace $k$ with $k + m$

## experiments
Simulation learned
-


NLP Task
- SST dataset
- X1 is BoW representation
- X2 is mean of random 300-dim word embeddings in sentence

Vision Task


## citation

```
@article{lee2020-predicting70,
    author = {Jason D. Lee, Qi Lei, Nikunj Saunshi and Jiacheng Zhuo},
    title = {Predicting What You Already Know Helps: Provable Self-Supervised Learning},
    year = {2020},
    booktitle = {arXiv preprint arXiv:2008.01064}
}
```
