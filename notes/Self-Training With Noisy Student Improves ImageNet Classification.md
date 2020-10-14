---
tags: [self-supervised]
title: Self-Training With Noisy Student Improves ImageNet Classification
created: '2020-10-08T22:44:18.024Z'
modified: '2020-10-09T00:10:47.227Z'
---

# Self-Training With Noisy Student Improves ImageNet Classification

## overview

- SOTA on supervised ImageNet 88.4 using semi-supervised 
- self-training with a bigger EfficientNet student, pseudo-labels, stochasticity


## Noisy Student Training
Algorithm
1. learn a teacher model on labelled images
2. use teacher to pseudo-label unlabelled images
3. learn **bigger** student model on joint labelled and pseudo-labelled images with **added noise**
4. make student the teacher and repeat from 2

Noising Student
- RandAugment on images
- dropout and stochastic depth in student model

Differences from previous work
- bigger teacher
- training jointly on pseudo-labels and labels
- using pseudo-labels from highly converged models
- noise injection
- model robustness evaluation

## experiments

ImageNet classification

Robustness




## citation

```
@inproceedings{xie2020-classification73,
    author = {Qizhe Xie, Minh-Thang Luong, Eduard Hovy and Quoc V. Le},
    title = {Self-Training With Noisy Student Improves ImageNet Classification},
    year = {2020},
    booktitle = {2020 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
    pages = {10687-10698},
    publisher = {IEEE}
}
```
