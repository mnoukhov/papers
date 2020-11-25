---
attachments: [Clipboard_2020-11-23-19-52-45.png]
title: Are all negatives created equal in contrastive instance discrimination?
created: '2020-11-24T00:40:38.125Z'
modified: '2020-11-24T02:53:48.532Z'
---

# Are all negatives created equal in contrastive instance discrimination?

## overview

- empirical study of properties of hard negative for SSL with MoCov2
- hardest 5\% of negatives are necessary and sufficient for perf
- hardest 0.1\% are unnecessary and can be detrimental

![](@attachment/Clipboard_2020-11-23-19-52-45.png)

## setup

contrastive instanced discrimination (CID) 
- *query* augmented view of image
- *positive* augmented view of same image
- *negative* augmented view of other image

[MoCo v2](@note/Momentum Contrast for Unsupervised Visual Representation Learning) on ImageNet
- SimCLR transforms
- ResNet-50 encodes *learned rep*
- MLP projects to *contrastive space embedding*
- InfoNCE

*negative difficulty* is just cosine distance between query and negative

## experiments

top 5% hard negatives are necessary and sufficient, others unnecessary
- using only top 5% difficult leads to same perf
- not using top 5% drastically drops perf
- not using easy has no change

removing top 0.1% slightly improves perf at lower temp

top 5% hard negatives are semantically similar
- usually same class
- if not same class, then higher wordnet similarity

easiest negatives are anti-correlated but also semantically similar
- a different class
- but close in wordnet
- but anti-correlated in dot product 

negatives under the model are both easier and harder than chance


## citation

```
@article{cai2020-discrimination68,
    author = {Tiffany Cai and Jonathan Frankle and David J. Schwab and Ari S. Morcos},
    title = {Are all negatives created equal in contrastive instance discrimination},
    year = {2020},
    booktitle = {arXiv preprint arXiv:2010.06682}
}
```
