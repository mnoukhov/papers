---
attachments: [Clipboard_2020-09-27-09-49-57.png, Clipboard_2020-09-27-09-51-15.png]
tags: [self-supervised]
title: Momentum Contrast for Unsupervised Visual Representation Learning
created: '2020-09-27T13:37:32.097Z'
modified: '2020-09-29T20:33:10.657Z'
---

# Momentum Contrast for Unsupervised Visual Representation Learning

## overview

- `MoCo` moment contrast builds dictionaries with contrastive SSL
- better than supervised ImageNet in 7 downstream tasks, closing supervised-unsupervised gap
- scales with data on 1B Instagram dataset

## MoCo

we can consider contrastive loss similar to dictionary lookup
- energy/score function is dot product of query $q$ and keys $k_{1,2 \ldots K}$
- higher score for $q \cdot k_{+}$ lower score for all others
- applied to [InfoNCE](@note/Representation Learning with Contrastive Predictive Coding) loss

![](@attachment/Clipboard_2020-09-27-09-49-57.png)

MoCo: momentum contrast

![](@attachment/Clipboard_2020-09-27-09-51-15.png)

- dictionary $K$ is a queue, newest batch added, oldest batch removed
- update query encoder $\theta_q$ with gradient descent
- update key encoder with query but momentum $\theta_k \gets m\theta_k + (1-m)\theta_q$

instance discrimination pretext task
- get two "views" for each image to make positive pairs
- each view is 224x244 random crop with image augmentation
- encode views (queries/keys) with ResNet to 128-dim 
- shuffling batch norm: shuffle key order (don't shuffle query) in batch to stop BN leaking info

## experiments

### linear classification on ImageNet 1M

MoCo is more efficient and better scaling
- accuracy increases with neg size $K$
- MoCo outperforms memory bank scaling to large $K$

momentum is necessary and needs to be slow
- momentum = 0 oscillates
- momentum $0.999$ is the sweet spot

MoCo gets SOTA on ImageNet
- scaling model from ResNet-50 to 4x width shows big improvements
- MoCo beats other methods with similar number of parameters at every size
- even better tuned results (MoCo v2) for ImageNet

### transferring features from ImageNet-1M and Instagram-1B

equalizing unsupervised and supervised finetuning
- normalized finetuning: finetune with BN, even in new layers
- same learning schedule as supervised, few epochs ($1x = 12, 2x =24$)

better than supervised ImageNet on PASCAL VOC object detection
- match performance with unsupervised ImageNet 1M
- outperforms with unsupervised Instagram 1B
- MoCo is always better than end-to-end and memory bank (they do well)
- MoCo is always better than Jigsaw, RelPos, etc...

better than supervised ImageNet on COCO (when trained on 2X)
- similar object detection, slightly worse with 1x finetuning
- better object detection with 2x finetuning
- better in 2x for keypoint detection, dense pose estimation too

on par or better than ImageNet pretrained for other task
- instance segmentation on Cityscapes
- semantic segmentation on Citscapes
- *slightly worse* than ImageNet supervised on semantic segmentation on PASCAL VOC


## citation

```
@inproceedings{he2020-representation80,
    author = {Kaiming He, Haoqi Fan, Yuxin Wu, Saining Xie and Ross Girshick},
    title = {Momentum Contrast for Unsupervised Visual Representation Learning},
    year = {2020},
    booktitle = {2020 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
    pages = {9729-9738},
    publisher = {IEEE}
}
```
