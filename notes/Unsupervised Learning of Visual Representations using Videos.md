---
attachments: [Clipboard_2020-09-17-10-05-21.png, Clipboard_2020-09-17-10-29-01.png, Clipboard_2020-09-17-11-53-45.png]
tags: [self-supervised]
title: Unsupervised Learning of Visual Representations using Videos
created: '2020-09-17T04:18:38.742Z'
modified: '2020-09-17T21:28:40.769Z'
---

# Unsupervised Learning of Visual Representations using Videos

## overview

- unsupervised video tracking as SSL
- siamese triplet network with ranking loss 
- unsupervised image tracking very close to ImageNet supervised

![](@attachment/Clipboard_2020-09-17-10-05-21.png)

## dataset: patch mining
100k Youtube videos into 8M image patches (pairs?)

get moving objects
- get SURF interest points and use IDT to get motion
- consider point moving if flow magnitude $> 0.5$
- filter out points with not enough motion (noise) or if there's too much (camera movement)

get bounding box
- first patch is 227x227 box that contain most SURF points
- second patch is 30 frames later tracked with KCF 

## model

![](@attachment/Clipboard_2020-09-17-10-29-01.png)

based on AlexNet

triplet loss
- keep same objects close in feature space, different objects far
- use cosine distance in feature space
- use gap parameter $M = 0.5$ to allow minimum distance diff (?)

![](@attachment/Clipboard_2020-09-17-11-53-45.png)

negative mining
- select random $K$ negative samples from the batch
- after 10 epochs do "hard negative mining" and use top $K$ with highest loss

finetuning options
1. "regular" reinit final linear layers, finetune
2. "iterative" finetune, re-SSL on triplets, finetune again

ensembling of models trained on 1.5M, 5M, 8M samples

## experiments

nearest neighbour on PASCAL VOC
- qualitative better than random results
- same semantic class for 40% of top 20

scene classification on MIT Indoor 67
- linear layer finetune beats random and SIFT
- still worse than ImageNet Alexnet

object detection on PASCAL VOC (finetune CNN, SVM on top)
- $44\%$ from scratch
- $47.5\%$ with SLL pretraining
- $47 \to 48\%$ using iterative finetuning on 5M
- $52\%$ using 3 ensemble

surface normal estimation NYUv2
- better than scratch, close to ImageNet pretrain

## citation 
```
@inproceedings{wang2015-representations93,
    author = {Xiaolong Wang and Abhinav Gupta},
    title = {Unsupervised Learning of Visual Representations Using Videos},
    year = {2015},
    booktitle = {2015 IEEE International Conference on Computer Vision (ICCV)},
    pages = {2794-2802},
    publisher = {IEEE}
}
```
