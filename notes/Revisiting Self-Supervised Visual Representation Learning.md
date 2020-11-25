---
tags: [self-supervised, vision]
title: Revisiting Self-Supervised Visual Representation Learning
created: '2020-09-10T22:09:54.768Z'
modified: '2020-11-17T03:53:16.860Z'
---

# Revisiting Self-Supervised Visual Representation Learning

## overview

- conduct large scale survey of vision SSL task and architectures
- show better SSL archs are not necessarily better supervised archs
- skip connection archs do not degrade in SSL
- perf improves with size and number of CNN filters
- linear probing evaluation is sensitive to SGD, tough to converge 

## setup

archs
- ResNet
- RevNet
- VGG

tasks
- [rotation](@note/Unsupervised Representation Learning by Predicting Image Rotations)
- [exemplar](@note/Discriminative Unsupervised Feature Learning with Exemplar Convolutional Neural Networks)
- [relative patch](@note/Unsupervised Visual Representation Learning by Context Prediction)
- [jigsaw](@note/Unsupervised learning of visual representations by solving jigsaw puzzles)

datasets
- Imagenet
- Places205

finetune pre-logits with linear layer

## experiments 

### archs and tasks

there is no best model across tasks
- Revnet works for rotation
- Resnet works for relative patch
- VGG seems bad all around

more CNN channels performs better
- results do extend across datasets (to Places205)

selecting correct arch and widening is sufficient for SOTA
- $\sim 15\%$ bump in accuracy
- Rotation is the best

### evaluation

linear evaluation is sufficient...to relatively evaluate models!
- MLP performs better but rankings are consistent with linear

pretext accuracy is a good indicator ...once architecture is fixed
- pretext acc is not good for choosing architecture for downstream task

SGD for linear evaluation is very sensitive
- can take a very long time and hard to replicate

### representations

skip connections prevent bad representations towards end of CNN
- earlier papers found pre-logit CNN layer to be too specialized
- residual connections seem to alleviate this problem

performance increases with model width and final rep size
- thin model goes $31\% \to 43\%$ with rep size
- works even in low data regime

## discussion

Extending and Analyzing SSL across Domains (Wallace et al, Arxiv 2020)
- 16 datasets with ResNet26
- pretext is indicative only if it requires semantic understanding (rotation on planes? no)

Can we use pretext performance for things like NAS? UnNas! (Arxiv 2020)
- contrary to this, yes, high correlation ($>80\%$) between true performance and pretext

## citation

```
@inproceedings{kolesnikov2019-representation43,
    author = {Alexander Kolesnikov, Xiaohua Zhai and Lucas Beyer},
    title = {Revisiting Self-Supervised Visual Representation Learning},
    year = {2019},
    booktitle = {IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
    pages = {1920-1929},
    publisher = {IEEE}
}
```
