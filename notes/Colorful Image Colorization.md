---
attachments: [Clipboard_2020-09-14-12-17-37.png, Clipboard_2020-09-14-12-50-03.png]
tags: [self-supervised, vision]
title: Colorful Image Colorization
created: '2020-09-14T15:52:43.031Z'
modified: '2020-09-15T20:48:12.663Z'
---

# Colorful Image Colorization

## overview

colorization
- produce more vibrant images by multimodality, reweighing for rare colours
- fool humans 32% of the time on colorization Turing test

SSL
- colorization "cross-channel encoding" as an SSL task
- SOTA on benchmarks

## colorization 

### training

task 
- given lightness $L$ predict other two channels $a$, $b$
- in CIE Lab Colorspace

![](@attachment/Clipboard_2020-09-14-12-17-37.png)

loss
- $L_2$ loss favours middle point of different modes, gray desaturated colours
- instead quantize colourspace into $Q = 313$ values
- learn categorical distribution over $Q$, treat as multinomial classification
- rebalance loss

class rebalance
- natural images have far more desatured colours, need to rebalance distribution
- take $p$ Imagenet distribution over $Q$
- smooth with a Gaussian filter $G_\sigma$
- mix with uniform distribution with weight $\lambda$
- take reciprocal and normalize so expected weight $w = 1$

### inference

map distribution over $Q$ to point in $ab$ space by taking *annealed mean*
- adjust softmax with temperature $T$
- too low $T$ is inconsistent coloring
- too high $T$ is desturated
- take mean of distribution with $T = 0.38$ 

## experiments

raw accuracy (AuC)
- percent of predicted colours within $L_2$ threshold   

semantic interpretability (VGG)
- pass colourized image to VGG for classification

perceptual realism (AMT)
- AMT guess which is real vs generated colorization

shows the *plausibility* isn't necessarily raw accuracy and their method works well for actual humans

![](@attachment/Clipboard_2020-09-14-12-50-03.png)

## SSL cross-channel encoding

use colorization as pretext task, finetune linear on Imagenet
- `conv1` is not competitive
- `conv2 +` are and show that colorization is a good pretext task

SOTA on unsupervised Pascal VOC classification
- demonstrates dataset generalization of features
- still short of supervised Imagenet


## discussion

CIE Lab colorspace avoid certain issues
- allows to separate grayscale from colour
- avoids chromatic abberations in green in RGB (see [[Unsupervised Visual Representation Learning by Context Prediction]] )

## citation

```
@inproceedings{zhang2016-colorization1,
    author = {Richard Zhang, Phillip Isola and Alexei A. Efros},
    title = {Colorful Image Colorization},
    year = {2016},
    booktitle = {European Conference on Computer Vision},
    pages = {649-666},
    publisher = {Springer, Cham}
}
```
