---
title: 'ADVENT: Adversarial Entropy Minimization for Domain Adaptation in Semantic Segmentation'
created: '2020-11-11T20:22:04.027Z'
modified: '2020-11-12T05:29:28.789Z'
---

# ADVENT: Adversarial Entropy Minimization for Domain Adaptation in Semantic Segmentation

## overview

- SOTA on unsupervised domain adaptation for semantic segmentation 
- pixel-wise entropy loss to penalize low-confident predictions on target domain
- entropy-based adversarial training for structure adaptation to target
- training with specific entropy ranges and class-ratio priors 

## entropy minimization

*direct entropy minimization* on target as part of auxiliary loss
- entropy for a pixel $h,w$ is $-\frac{1}{\log C} \sum^C P(h,w,c) \log P(h,w,c)$
- entropy loss $L_{ent}$ is sum of entropies
- total loss is $L_{seg}(\text{source}) + L_{ent}(\text{target})$

*adversarial training* for implicit entropy minimization
- minimize source-target distance in entropy ("weighted self-information space")
- self-information map vector  $I(h,w) = - P(h,w) \log P(h,w)$
- GAN with $\lambda$-weighted discriminator that takes $I$ as input and predicts source or target

class-ratio prior auxiliary loss
- prior number of pixels per class, on source $L_1$ normalized
- penalize if target has too few compared to prior ($\mu$-weighted)

## setup

synthetic-2-real UDA
- SYNTHIA or GTA5 source
- Cityscapes target, 2975 unlabelled used for training

segmentation model
- Deeplab-V2 base 
- Atrous Spatial Pyramid Pooling 
- VGG16 or ResNet-101

adversarial network
- DCGAN
- 4 conv, leakyReLU, fc

baselines
- FCN in the Wild
- Adapt SegMap (+ ResNet 101)
- Self-training (+ class balancing)

## results

SOTA on GTA5 $\to$ Cityscapes
- MinEnt is comparable to SOTA with VGG, but Adv is better!
- small diff but still SOTA with ResNet
- ensemble MintEnt + Adv gets strong SOTA with ResNet

SOTA on SYNTHIA $\to$ Cityscapes
- VGG MinEnt already achieves strong SOTA and class-balance does even better
- ResNet Adv is necessary for SOTA and ensemble again is best

ablations
- MinEnt training on 30% high-entropy (most confusing) helps with ResNet
- class ratio effect is a bit iffy imo
- object detection is possible but Faster-RCNN still better

## citation 

```
@inproceedings{vu2019-minimization23,
    author = {Tuan-Hung Vu and Himalaya Jain and Maxime Bucher and Matthieu Cord and Patrick Perez},
    title = {ADVENT: Adversarial Entropy Minimization for Domain Adaptation in Semantic Segmentation},
    year = {2019},
    booktitle = {2019 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
    pages = {2517-2526},
    publisher = {IEEE}
}
```
