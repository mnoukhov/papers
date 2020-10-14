---
attachments: [Clipboard_2020-09-27-21-06-10.png]
tags: [self-supervised]
title: A Simple Framework for Contrastive Learning of Visual Representations
created: '2020-09-27T19:34:20.544Z'
modified: '2020-09-29T21:25:16.265Z'
---

# A Simple Framework for Contrastive Learning of Visual Representations

## overview

- SimCLR gets new SOTA on SSL ImageNet, matches supervised

show useful tricks for contrastive SSL
- composing data augmentations
- learnable non-linear transform
- normalized embeddings, appropriate temperature
- larger batch sizes, longer training, wider nets

## SimCLR

![](@attachment/Clipboard_2020-09-27-21-06-10.png)

stochastic data augmentation $x \to \tilde x_i, \tilde x_j$
- cropping (and resize)
- color distortion
- Gaussian blur

base encoder ResNet $f$

projection head $g$ is 2-layer MLP with ReLU

contrastic loss function on $z_{i,j}$
- use other $2(N-1)$ examples as negative samples
- normalized temperature cross-entropy (NT-Xent) loss with cosine similarity score $sim(z_i,z_j)$

large batch size ($256 \to 8192$) training
- LARS optimizer 
- aggregate BN statistics over all GPUs to prevent leakage (similar to [MoCo](@note/Momentum Contrast for Unsupervised Visual Representation Learning) shuffling)

experiments on ImageNet, linear evaluation

## data augmentation

predictive task can instead be defined with data augmentation

no *single* augmentation is sufficient for good performance
- apply one type of data augment for $\tilde x_i$ another for $\tilde x_j$
- best results are with color-crop or crop-color, but still not super performant

stronger color augmentation works better for SSL
- performance is linear with color distortion strength
- inverse linear for supervised learning
- AutoAugment isn't better than cropping + strong color distortion

## encoder/head archs

SSL benefits from wider-deeper nets (even more than supervised)

use non-linear projection head...but use the representation before it!
- final representation must learn to be invariant to distortions, might be bad for downstream
- $h$ contains more info about transformation than $g(h)$

## loss function, batch size

NT-Xent better than alternatives (logistic, max-margin triplet)
- $L_2$ norm (cosine similarity) + temperature weighs samples 
- other loss don't weigh negatives by hardness, need semi-hard negative mining
- $L_2$ is worse on contrastive task but better on downstream

constrative needs bigger batches, longer training
- larger batch sizes are better with short training ($<100$ epochs)
- with more training steps, smaller batch sizes catch up if resampled/shuffled batches

## SOTA

SOTA on unsupervised ImageNet
- $5\%+$ better on top-1 than CPCv2
- matches supervised ResNet-50 performance

better on semi-supervised ImageNet ($1\%, 10\%$)
- beats hand-crafted data augmentation SOTA

transfer learning as good or better than supervised on natural images
- linear evaluation mostly better or same than supervised
- full finetune almost always better or same than supervised


## SimCLRv2

smaller data regimes benefit from bigger models

big representations can be compressed
- self-distillation with unlabelled data gets nearly same perf with 4x less params 

deep projection head is even better
- bigger, deeper MLP

## citation

```
@inproceedings{chen2020-representations91,
    author = {Ting Chen, Simon Kornblith, Mohammad Norouzi and Geoffrey Hinton},
    title = {A Simple Framework for Contrastive Learning of Visual Representations},
    year = {2020},
    booktitle = {ICML 2020: 37th International Conference on Machine Learning}
}
```

v2

```
@article{chen2020big,
  title={Big Self-Supervised Models are Strong Semi-Supervised Learners},
  author={Chen, Ting and Kornblith, Simon and Swersky, Kevin and Norouzi, Mohammad and Hinton, Geoffrey},
  journal={arXiv preprint arXiv:2006.10029},
  year={2020}
}
```

