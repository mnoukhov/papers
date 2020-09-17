---
attachments: [Clipboard_2020-09-16-17-44-54.png]
tags: [self-supervised]
title: 'Shuffle and Learn: Unsupervised Learning using Temporal Order Verification'
created: '2020-09-16T21:40:09.145Z'
modified: '2020-09-17T04:27:31.828Z'
---

# Shuffle and Learn: Unsupervised Learning using Temporal Order Verification

## overview

- SSL task of whether video frames are in order
- show this improves action recognition and human pose estimation
- outperforms learning from scratch and certain supervised pretrain

## shuffle and learn

![](@attachment/Clipboard_2020-09-16-17-44-54.png)

task: tuple verification
1. extract a 3-tuple of image frames
2. ask whether the frames are in order

sampling
- choose 5 ordered image frames with high motion (magnitude of optical flow)
- $2, 3, 4$ is a positive sample (and its inverse)
- $2, 1¸ 4$ and $2, 5, 4$ are negative
- enforce $3 != 1 or 5$ with SSD on RGB

model
- pass each frame through CaffeNet (modification of Alexnet) with batch norm
- concat features and pass through linear 
- cross-entropy on ordered or not

finetuning test time
- 25 frames uniformally samples
- 5 crops, 2 flips per frame
- output is average prediction over 25 * 5 * 2 frames

## experiments

### Action Recognition UCF-101

finetuning on UCF-101 with extra linear layer
- better when we have bigger positive sample window (bigger difference between $2$ and $4$)
- worse when we have bigger negative sample window (bigger $\min(2 - 1, 5 - 4)$)
- best negative-positive sampling ratio is 75-25

nearest neighbour retrieval
- pretrained imagenet focuses on scene semantics
- SSL focuses on human pose (makes sense given pretrain dataset)

Shuffle and Learn gets SOTA on action recognition 
- beats training from scratch
- beats temporal smoothness and [[Unsupervised Learning of Visual Representations using Videos]]
- finetuning on SLL improves supervised ImageNet model 

### Pose Estimation FLIC, MPII

Unsupervised SOTA
- outperforms *supervised UCF* $7.6\%$ on FLIC

Supervised SOTA after finetuning 
- finetuning ImageNet model with Tuple Verification


## citation

```
@inproceedings{misra2016-unsupervised69,
    author = {Ishan Misra, C. Lawrence Zitnick and Martial Hebert},
    title = {Shuffle and Learn: Unsupervised Learning Using Temporal Order Verification},
    year = {2016},
    booktitle = {European Conference on Computer Vision},
    pages = {527-544},
    publisher = {Springer, Cham}
}
```
