---
attachments: [Clipboard_2020-09-23-20-47-31.png, Clipboard_2020-09-23-21-06-57.png]
tags: [self-supervised]
title: Contrastive Multiview Coding
created: '2020-09-24T00:34:01.498Z'
modified: '2020-09-25T00:22:32.990Z'
---

# Contrastive Multiview Coding

## overview

- contrastively maximize MI between image channel view of images
- match SOTA on SSL
- show contrastive objective outperforms cross-view coding

![](@attachment/Clipboard_2020-09-23-20-47-31.png)

## CMC

motivation
- good information is shared across views
- predictive learning would guess chrominance given luminance
- contrastive learning differentiates between positive and negative pairs of (chrominance,luminance)

2-view CMC
- embed each view $k$ with $f_{\theta_k}$
- calculate score (cosine similarity) $h_\theta$ with temperature $\tau$
- calculate contrast loss from each view's perspective
![](@attachment/Clipboard_2020-09-23-21-06-57.png)
- combine losses $L(V_1, V_2) = L^{V_1, V_2}_{contrast} + L^{V_2, V_1}_{contrast}$

multiview CMC
- core-view chooses one main view $V_1$, $L_C = \sum_{j=2}^M L(V_1, V_j)$
- full-graph $L_F = \sum_{1 \leq i \leq j \leq M} L(V_i, V_j)$

contrastive loss uses $m-1$ negative samples and does softmax classification
- instead of NCE for some reason

## experiments

image CMC on luminance and chrominance
- two colorspaces $L,ab$, $Y, Db,Dr$
- ResNet-50 encoders
- "comparable to" SOTA on ImageNet using $Y, DbDr$, and RandAugment 

video CMC on ventral (objects) and dorsal (motion)
- ventral contrastive on consequetive frame $i_t, i_{t+k}$
- dorsal contrastive on optical flow of 10 frames centered at $t$, $i_t, f_t$
- encoded using CaffeNet (??) 
- using both views (V+D) achieves SOTA on unsupervised UCF-101

more views with 3D images NYU-Depth
- luminance, chrominance, depth, surface normal, semantic labels
- randomly crop patches to increase negative sampling
- lots of other dumb tricks

core-view using luminance shows number of view matter
- better than random, close to supervised
- acc improves with number of views

full-graph shows not much improvement over core-view

contrastive beats predictive on different pairs
- trained on NYU-Depth but evaluated on STL-10

mutual information experiments
- compared CMC for similar views and different views, measured by MI (using MINE)
- with image patches close has high MI, far has low MI. Highest acc in the middle
- with different color spaces, lowest MI provides highest accuracy
- MI does not necessarily help selecting views

## discussion

What makes for good views for contrastive learning (Tian et al, 2020?)
- data augmentation makes a big improvement
- you want views that provide an invariance useful for your downstream task
- view generator to make best view for downstream task


## citation

```
@misc{tian2019contrastive,
    title={Contrastive Multiview Coding},
    author={Yonglong Tian and Dilip Krishnan and Phillip Isola},
    year={2019}
}
```
