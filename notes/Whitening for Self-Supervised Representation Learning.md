---
attachments: [Clipboard_2020-11-16-21-40-06.png]
title: Whitening for Self-Supervised Representation Learning
created: '2020-11-17T02:28:59.831Z'
modified: '2020-11-17T03:08:42.930Z'
---

# Whitening for Self-Supervised Representation Learning

## overview

- avoid negative sampling in SSL with whitening transform projecting input onto sphere
- scatter representations on sphere, use MSE on distance between positive pairs
- competitive with BYOL, contrastive methods on CIFAR, STL, tiny ImageNet

![](@attachment/Clipboard_2020-11-16-21-40-06.png)

## whitening

get positive samples with data augment
- image cropping
- grayscaling
- color jitter

whitening MSE
- $e$ encode images with ResNet-18 + avg-pool
- $g$ one fc layer projection + bn
- split batch into $d$ sub-batch slices
- $f$ whitening normalizes slice mean to 0, covariance to identity
- MSE loss on positive pairs


## experiments

baselines
- [SimCLR](@note/A Simple Framework for Contrastive Learning of Visual Representations)
- [BYOL](@note/Bootstrap Your Own Latent A New Approach to Self-Supervised Learning)

linear probing and 5-nn on
- CIFAR-10,100
- Tiny ImageNet (100k train, 10k test, 200 classes)
- STL-10



## notes

advantage over contrastive
- negative sampling can force similar (but neg sampled) to be far apart
- batch-norm can't be used for circumventing loss bc no learnable layers on top of whitening* ?

no experiments on ImageNet??

no experiments showing speed / advantage of 




## citation

```
@article{ermolov2020-representation97,
    author = {Aleksandr Ermolov and Aliaksandr Siarohin and Enver Sangineto and Nicu Sebe},
    title = {Whitening for Self-Supervised Representation Learning.},
    year = {2020},
    booktitle = {arXiv preprint arXiv:2007.06346}
}
```
