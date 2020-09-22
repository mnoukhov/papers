---
attachments: [Clipboard_2020-09-20-17-14-12.png]
tags: [self-supervised, vision]
title: Unsupervised Learning by Predicting Noise
created: '2020-09-17T23:07:30.039Z'
modified: '2020-09-22T01:21:58.513Z'
---

# Unsupervised Learning by Predicting Noise

## overview

- map each image in a batch to a random point on a sphere
- discriminative, unsupervised, efficient objective
- on par with  unsupervised Pascal and ImageNet

![](@attachment/Clipboard_2020-09-20-17-14-12.png)

## Noise as Targets (NAT)

Jointly learn representations $y = f_\theta(x)$ and mapping $\theta$
- map deep features to a uniform distribution over a $d$-dimensional $L_2$ sphere

NAT
- choose $k > n$ targets uniformally on $L_2$ unit sphere, $C$
- each image is assigned $P$ to a different target and each target assigned only once
- learn mapping using $L_2$ loss 

$\max_\theta \max_{P \in \mathbb{P}} Tr(PCf_\theta(X)^T))$

Optimizing objective with stochastic updates to reduce complexity
- per batch, compute features $f_\theta(X_b)$
- compute $P*$ for the batch using Hungarian algorithm $O(b^3)$
- compute $\nabla_\theta L(\theta)$ using $P*$ and backprop

## setup

AlexNet arch with MLP on top layer transfer

Data augmentation
- image gradients to avoid trivial solutions (clustering colours)
- random cropping and flipping as used in AlexNet

Update assignment $P$ (with $P*$) every 3 epochs

Datasets
- ImageNet classification
- Pascal VOC classification and detection

## experiments

### design choices

Square loss justified over softmax
- softmax only slightly better than square loss
- performance improvements are likely worth it

Image preproc and augmentations are feasible
- only slightly degrades supervised performance
- data augmnetation can help SSL but doesn't remove useful info

Continuous representation better than discrete (1-hot classes)

Learning improves with time but saturates

Updating $P$ optimal for every 3 epochs 
- fixed $P$ is $10\%$ worse

### imagenet and pascal transfer
qualitative
- conv1 features for unsupervised are less sharp but similar to supervised
- nearest neighbour captures some semantic information

imagenet transfer, not SOTA but good
- better than all unsupervised (e.g. BiGAN)
- worse than Jigsaw (SSL) but that one has many more features
- much worse than SIFT + Fischer Vectors (best handcrafted)

pascal 
- better than SOTA unsupervised (BiGAN)
- comparable to SSL

## citation 

```
@inproceedings{bojanowski2017-unsupervised52,
    author = {Piotr Bojanowski and Armand Joulin},
    title = {Unsupervised learning by predicting Noise},
    year = {2017},
    booktitle = {ICML'17 Proceedings of the 34th International Conference on Machine Learning - Volume 70},
    pages = {517-526},
    publisher = {JMLR.org}
}
```
