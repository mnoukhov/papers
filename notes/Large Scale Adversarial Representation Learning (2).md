---
attachments: [Clipboard_2020-11-19-14-22-34.png]
title: Large Scale Adversarial Representation Learning (2)
created: '2020-11-19T19:14:51.473Z'
modified: '2020-11-19T19:46:09.976Z'
---

# Large Scale Adversarial Representation Learning

## overview

- scaling up BiGAN + BigGAN, better joint discriminator
- matches SOTA on ImageNet
- high quality image generation

![](@attachment/Clipboard_2020-11-19-14-22-34.png)

## BigBiGAN
model
- generator $G = P(x|z)$
- encoder $E = P(z|x)$
- joint discriminator between $(x,\hat z)$ and $(\hat x, z)$

extra discriminator loss terms to guide 
- $F$ scores $x,\hat x$
- $H$ scores $z, \hat z$
- $J$ scores $F(x),H(z)$ pair
- $l_{D} = \sum_{t \in F,H,Z} \max(0, 1 - y t)$ where $y \in \{-1,+1\}$ 
- same JSD GAN loss with $l_D$

## experiments

metrics
- linear probing for representation
- inception score (IS), Frechet Inception Distance (FID) for generation


Imagenet linear probing SOTA
- matches efficient CPC
- 4xRevNet > ResNet
- BN+CReLU feature is slightly better $[ReLU(BN(output)), ReLU(-BN(output))]$

SOTA on image generation
- beats BigGAN cleanly, getting better with more steps
- extra loss terms help
- lighter image augmentation than rep learning

## citation

```
```
