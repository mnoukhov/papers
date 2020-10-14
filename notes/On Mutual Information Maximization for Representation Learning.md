---
tags: [self-supervised]
title: On Mutual Information Maximization for Representation Learning
created: '2020-09-30T16:28:56.241Z'
modified: '2020-09-30T17:21:54.092Z'
---

# On Mutual Information Maximization for Representation Learning

## overview

- SSL methods can't explain success with MI, encoder bias and estimator form might
- tighter MI bounds do worse
- deep metric learning may be a better explanation

## background

practical InfoMax: given two views, maximize MI between using estimator I
- estimation allows us to be in lower-dimensional space
- two views can capture different aspects/modalities

"if a classifier can distinguish between samples from joint $p(x,y)$ and marginals $p(x)p(y)$ then X and Y have high MI"
- [InfoNCE](@note/Representation Learning with Contrastive Predictive Coding)
- variation KL (NWJ, 2010)

## MI is not a good explanation

setup
- MNIST images with views being top half and bottom half
- downstream linear evaluation on top representation $g_1(x_{top})$
- bilinear critic $f(x,y) = x^T W y
- baseline linear on pixel space $x_{top}$ gets 85%

### large MI is not predictive of downstream

realNVP encoder, comparing $I_{NCE}, I_{NWJ}$

bijective encoders still improve downstream
invertible encoders with high MI can be worse than raw pixels

estimators bias encoder to be hard to invert

simple critics with loose bounds can do better than high capacity critics with tighter bounds

optimizing estimators for the same MI value shows great improvement




## citation

```
@inproceedings{tschannen2020-representation8,
    author = {Michael Tschannen, Josip Djolonga, Paul K. Rubenstein, Sylvain Gelly and Mario Lucic},
    title = {On Mutual Information Maximization for Representation Learning},
    year = {2020},
    booktitle = {ICLR 2020 : Eighth International Conference on Learning Representations}
}
```
