---
attachments: [Clipboard_2020-11-19-14-22-34.png]
title: Large Scale Adversarial Representation Learning
created: '2020-11-19T19:14:51.473Z'
modified: '2020-11-19T19:33:52.564Z'
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
- same G

## citation

```
```
