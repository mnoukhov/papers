---
attachments: [Clipboard_2020-09-21-21-59-46.png]
tags: [self-supervised, vision]
title: 'A critical analysis of self-supervision, or what we can learn from a single image'
created: '2020-09-22T01:22:32.913Z'
modified: '2020-09-22T19:34:46.190Z'
---

# A critical analysis of self-supervision, or what we can learn from a single image

## overview

- SSL can learn first few conv layers from a single image + augmentation
- SSL can't learn later layers as well as supervised even with large data
- characterizes current deficiencies in SSL

## method

augmented dataset
- compare to method trained on $d$ images
- dataset contains $N$ true images with $d - N$ augmentations

augmentations
- get image patches of min size $\beta H W$
- rotate by max 35 degrees
- flip with prob 0.5
- color jitter, and color rescale
- rescale to standard size

for $N=1$, used each of the images below
![](@attachment/Clipboard_2020-09-21-21-59-46.png)

methods
- BiGAN
- [RotNet](@note/Unsupervised Representation Learning by Predicting Image Rotations)
- DeepCluster, uses $k$-means to learn pseudo-labels and learns mapping similar to [NAT](@note/Unsupervised Learning by Predicting Noise)

AlexNet base architecture 

## quantitative experiments

linear probes on different layers for ImageNet, CIFAR-10,100

data augmentation
- without data augment is essentially random network
- scale is most important 
- all three augments are necessary in very low data regime

imagenet
- a single image is better than random for all layers
- very close to SOTA for `conv2` with one image (31.5, 32.5)
- RotNet needs more images, and prefers natural images
- augmentations can compensate for a plain, uninteresting image (C)

more than one image 
- BiGAN fails to converge for N>1
- RotNet and DeepCluster improve in later layers but `conv1,2` one is enough
- overall `conv1,2` and somewhat `3` do not improve from $1 \to 1M$ images

cifar-10,100
- GAN better than supervised for `conv1-3`
- single-image GAN nearly as good
- *single image good enough for first layers but not for later layers*

## qualitative experiments

conv1 features visuals look ok
- unsupervised closely resemble supervised
- good looking edges (RotNet) aren't better than redundant weird ones (DeepCluster)

finetuning with frozen `conv1,2` works well
- worse than supervised but not bad
- learned features are not limiting to the network

neural style transfer is just as good
- visually similar to using full supervised on imagenet
- early layers are sufficient for style transfer

## citation

```
@inproceedings{asano2020-supervision36,
    author = {Y M Asano, C Rupprecht and A Vedaldi},
    title = {A critical analysis of self-supervision, or what we can learn from a single image},
    year = {2020},
    booktitle = {ICLR 2020 : Eighth International Conference on Learning Representations}
}
```
