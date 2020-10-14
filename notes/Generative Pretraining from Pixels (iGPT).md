---
attachments: [Clipboard_2020-10-05-12-11-04.png]
tags: [self-supervised, vision]
title: Generative Pretraining from Pixels (iGPT)
created: '2020-10-05T16:00:06.814Z'
modified: '2020-10-06T22:21:40.850Z'
---

# Generative Pretraining from Pixels (iGPT)

## overview

- generative SS pretraining for images using GPT
- SOTA on low-res (CIFAR,STL) despite not using CNN!
- competitive on ImageNet but uses more params, compute

## data preprocessing

preprocessing dataset images
- resize to allow 224x224 crop on ImageNet
- resize to allow 32x32 on CIFAR

preprocessing to sequence
- resize to input resolution ($32^2, 48^2, or 64^2$)
- 9-bit color palette (using k-means clustering) to reduce RGB to scalar

## iGPT
![](@attachment/Clipboard_2020-10-05-12-11-04.png)

generative modelling of image, sequence of patches $x_1, \ldots x_n$
- view images as a sequence (raster order)
- **autoregressive** minimize $p(x_n | x_1 \ldots x_{n-1}, \theta)$
- **BERT** mask each with prob 0.15, guess masked given unmasked

evaluation
- linear probing from learned features
- finetuning on cross-entropy loss + also generative modelling loss

architecture
- XL is 60 layers, dim 3072, 6.8B params and trained on huge online dataset
- L is 48 layers, dim 1536, 1.4B params
- M is 36 layers, dim 1024, 455M params
- S is 24 layers, dim 512, 76M params

## experiments

### linear probing with autoregressive
best linear probe layer is in the middle
- using residuals, so issue is not it being stuck
- generative modelling suffers from overspecialization to task in later layers
- whereas early layers have good global 

better SSL performance and bigger models do better 

SOTA on CIFAR-10, CIFAR-100, STL-10
- despite pretraining on CIFAR not ImageNet

comparable on ImageNet [CPCv2](@note/Representation Learning with Contrastive Predictive Coding) but worse than [SimCLR](@note/A Simple Framework for Contrastive Learning of Visual Representations)
- since using $d = 3072$ compared to usual $8192$, concat 5 layer features

linear probing on low-data CIFAR-10 
- iGPT-L beats some other SSL approaches on 250 labels, 4000 labels
- does pretty badly on 40 labels as it likely memorizes them

### finetuning

unsupervised transfer 
- SOTA on CIFAR-10,100
- slightly worse on ImageNet
- unsupervised initialization better than baseline, achieves same $53.2\%$ after 1 epochs vs 18

BERT objective
- linear probing is worse everywhere
- finetune nearly as good on CIFAR-10,100
- finetune better on ImageNet!
- accounting for masked distribution difference gets 1% boost

## discussion

not fully tractable with image sizes
- preprocessing scheme was necessary
- 2x,3x params and compute
- replace dense self-attention?

## citation

```
@inproceedings{chen2020-pretraining21,
    author = {Mark Chen, Alec Radford, Rewon Child, Jeffrey K Wu, Heewoo Jun, David Luan and Ilya Sutskever},
    title = {Generative Pretraining From Pixels},
    year = {2020},
    booktitle = {ICML 2020: 37th International Conference on Machine Learning}
}
```
