---
title: Self-supervised learning through the eyes of a child
created: '2020-11-25T21:53:41.185Z'
modified: '2020-11-26T02:09:22.890Z'
---

# Self-supervised learning through the eyes of a child

## overview

- SSL on longitudinal egocentric child videos (SAYCam) learns good high-level rep
- New SSL using video temporal invariance
- Labelled dataset from SAYCam to evaluate SSL methods

## setup 

SAYCam
- 500 hours of egocentric audio-video
- 3 children, 6-32mo, 1-2 hours/week
- 30/24 fps, 640x480

SAYCam eval
- human-transcribed child S videos
- 58k frames in 26 classes (sand, crib, etc)

Toybox dataset (Wang et al, 2018)
- 12 toys types (ball, car, etc), 30 each
- video of natural transformation for 20sec
- 84k frames when sampled 1 fps

training
- MobileNetV2 $to$ 1280 embed
- one model per child

temporal invariance SSL
- split all video into *episodes*
- predict episode given frame

## models

best temporal invariance hyperparams
- 5fps frame rate
- 288sec episode length
- color + grayscale data augment like [SimCLR](@note/A Simple Framework for Contrastive Learning of Visual Representations)

baselines
- non-temporal [MoCoV2](@note/Momentum Contrast for Unsupervised Visual Representation Learning)
- temporal contrastive (next/prev frame are positive, others negative)
- MobileNetV2
  - untrained
  - ImageNet pretrained 
- HOG shallow baseline

## experiments

SOTA linear eval on SAYCam-labelled
- temporal SSL works well, always beats MoCo
- generalization from children A,Y $\to$ labelled S
- better perf with higher sampling (5fps), longer segment, data augment

better than contrastive, worse than ImageNet on Toybox
- not huge margin with MoCo

linear binary classification *car* vs *road*, *door* vs *window* etc.
- temporal SSL on S sometimes beats ImageNet pretrained

qualitative shows decent perf
- spatial attention maps on cats find cats
- class selectivity of features gets higher in higher layers

## limitations

- data is only 2 weeks compared to 2 years of actual life
- model ignores embodied aspect
- audio data isn't used 
- data augmentation isn't realistic

## citation

```
@inproceedings{orhan2020-supervised38,
    author = {A. Emin Orhan and Vaibhav V. Gupta and Brenden M. Lake},      
    title = {Self-supervised learning through the eyes of a child},
    year = {2020},                                                               
    booktitle = {Neural Information Processing Systems}                                                                                               
}
```
