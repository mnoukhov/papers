---
attachments: [Clipboard_2020-10-13-12-13-15.png, Clipboard_2020-10-14-10-01-46.png]
tags: [self-supervised, semi-supervised, vision]
title: 'FixMatch: Simplifying Semi-Supervised Learning with Consistency and Confidence'
created: '2020-10-13T15:49:14.062Z'
modified: '2020-10-14T14:56:07.253Z'
---

# FixMatch: Simplifying Semi-Supervised Learning with Consistency and Confidence

## overview

- pseudo-labelling + consistency regularization 
- pseudo label on weak augment, predict label on strong augment
- FixMatch achieves SOTA on semi-supervised CIFAR-10

![](@attachment/Clipboard_2020-10-14-10-01-46.png)

## FixMatch

*pseudo-labelling* (aka self-training) trains using model's predictions as labels
*consistency regularization* model should output same label for two different augments of same input 

$Loss_{fm} = loss_{sup} + \lambda_u loss_{unsup}$

![](@attachment/Clipboard_2020-10-13-12-13-15.png)
- $\mu B$ is unsup batch size
- $\tau$ is threshold above which you retain pseudo-label
- $\alpha$ is weak augment
- $\mathcal{A}$ is strong augment
- $q_b$ is pseudo-label
- $\hat q_b$ is model prediction

augmentation weak
- randomly flip and shift 
augmentation strong
- Cutout + RandAugment or CTAugment
- RandAugment: sample severity magnitude from range during training
- CTAugment: sample 2 transforms, learn severity weight online

standard SGD with momentum 
- cosine learning rate decay

## experiments
Compared to Mean Teacher, UDA, ReMixMatch (prev SOTA)


SOTA on semi-supervised low-data 
- SOTA CIFAR10 on all regimes and especially on 4 labels/class
- close to SOTA CIFAR100, best on 100 labels/class (using distribution alignment from ReMix, gets SOTA on CIFAR100)
- SOTA on STL-10 1000-label
- close to SOTA on 10% labelled ImageNet (SOTA uses second phase learning, likely could get SOTA if used same second phase)
- shows that in low data regime, the example quality is very important: more prototypical examples give huge boost in quality

## ablations

sharpening prediction distribution with temp $T$ is like a soft version of pseudo-labelling
- introduces a new hyperparam but does no better in performance when using thresholding $\tau$

augmentations are important
- Cutout + CTAugment are both necessary
- weak / weak, strong / strong both fail, weak/strong is necessary

more unlabelled data helps
- scaling learning rate linearly with batch size

SGD + momentum beats ADAM
- learning rate decay helps slightly (linear or cosine)

weight decay is very necessary


## citation

```
@article{sohn2020-simplifying98,
    author = {Kihyuk Sohn and David Berthelot and Chun-Liang Li and Zizhao Zhang and Nicholas Carlini and Ekin D. Cubuk and Alex Kurakin and Han Zhang and Colin Raffel},
    title = {FixMatch: Simplifying Semi-Supervised Learning with Consistency and Confidence},
    year = {2020},
    booktitle = {arXiv: Learning}
}
```
