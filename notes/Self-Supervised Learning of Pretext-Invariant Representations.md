---
attachments: [Clipboard_2020-11-16-22-19-27.png]
tags: [self-supervised, vision]
title: Self-Supervised Learning of Pretext-Invariant Representations
created: '2020-11-17T03:15:15.130Z'
modified: '2020-11-17T05:52:15.121Z'
---

# Self-Supervised Learning of Pretext-Invariant Representations

## overview

- PIRL: use SSL pretext task as data augmentation and learn invariance to it
- PIRL with Jigsaw task beats some other SSL tasks and competitive with AMDIM with fewer params
- I don't think using SSL pretext tasks make for good data augmentation, MoCo v2 beats this while being simpler and smarter


![](@attachment/Clipboard_2020-11-16-22-19-27.png)

## PIRL

learn invariance to image transform + perturb
- regular SSL tranform tasks like [Jigsaw](@note/Unsupervised learning of visual representations by solving jigsaw puzzles) learn to predict perturbation (e.g. reverse jigsaw scrambling, guessing rotation)
- this requires constructing a presentation *covariant* to perturbation
- we instead want *invariant* to perturbation (e.g. same output regardless of rotation)

PIRL
- embed image $I$, transformed $I_t$ to representation $v_I,v_{T^t}$ with ResNet-50
- project $f(v_I)$ and $g(v_{I^t})$ linear layers
- $N$ negative samples $m_{I'}$ from momentum memory bank (similar to [MoCo](@note/Momentum Contrast for Unsupervised Visual Representation Learning)?)
- get similarity $s$ to calculate NCE loss

loss $L = \lambda L_{NCE}(m_I, g(v_{I^t}) + (1 - \lambda) L_{NCE}(m_I, f(v_{I})$ 
- first part is regular NCE but using $m_I$ instead of $f(v_I)$  
- second part should make $f(v_I)$ and memory bank $m_I$ closer

PIRL with Jigsaw
- extract 9 patches
- embed each patch, avg pool, linear project
- concat patch representation to get $v_{I}$
- linear projection $g$

## experiments

pretrain on ImageNet
- batch size 1024
- 32k negative samples

object detection on PASCAL with Faster R-CNN
- beats Jigsaw by decent margin
- worse than MoCo v1

classification on ImageNet
- beats other simple SSL ResNet-50 models including Jigsaw
- competitive with CMC with ensembling
- competitive with AMDIM when doubled channels (but fewer params) 

classification VOC, Places205, iNaturalist2018
- does well but who cares

semi-supervised ImageNet 1% 10%
- competitive with other methods

noisy pretraining on YFCC
- seems to be better than other, very simple methods

analysis
- representations do seem to be invariant
  - whereas Jigsaw model is covariant
- res5 layer gives best representation
  - doesn't decay with depth as in tasks that are not similar (e.g. Jigsaw)
- $\lambda = 0.5$ best for ImageNet
- more data augmentation is better
- more negative samples are better
- can work with rotation too, but not as well


## citation

```
@inproceedings{misra2020-representations1,
    author = {Ishan Misra and Laurens van der Maaten},
    title = {Self-Supervised Learning of Pretext-Invariant Representations},
    year = {2020},
    booktitle = {2020 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
    pages = {6707-6717},
    publisher = {IEEE}
}
```
