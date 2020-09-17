---
attachments: [Clipboard_2020-09-08-15-43-07.png, Clipboard_2020-09-08-16-01-02.png]
tags: [self-supervised, vision]
title: Unsupervised Visual Representation Learning by Context Prediction
created: '2020-09-08T19:34:30.801Z'
modified: '2020-09-10T22:17:26.932Z'
---

# Unsupervised Visual Representation Learning by Context Prediction

## overview

- use spatial context as self-supervised signal
- predict position of second image patch relative to first
- unsupervised discovery of objects on Pascal VOC 
- transfer learning for RCNN achieves SOTA

![](@attachment/Clipboard_2020-09-08-15-43-07.png)

## setup

Generate patch dataset from Imagenet
- sample first patch uniformally
- sample second patch from one of 8 neighboring patches
- preprocess with mean subtraction, projecting/dropping colours, downsampling

Avoid trivial solutions
- include a gap between patches to avoid matching texture
- randomly jitter each patch up to 7 pixels to avoid matching long lines
- chromatic aberration of lenses can be learned by convnets !?! randomly drop 2/3 colour channels from the image patch, replacing with Gaussian noise


Train model on predicting which of 8 positions the second patch belongs to

![](@attachment/Clipboard_2020-09-08-16-01-02.png)

## experiments

### nearest neighbour

NN finds better semantic matches than random or imagenet-trained Alexnet 

### object detection

Pretraining with this scheme leads to good improvements on VOC
- from scatch is worse than Alexnet
- but pretrained on Imagenet and finetune outperforms Alexnet from scratch
- still worse than supervised Alexnet pretraining on Imagenet

### visual data mining
Find image clusters from trained features
- take four patches from an image (for redundancy)
- find top 100 images that have highest match with the four patches
- geometrically verify that matching patches are consistent

Unsupervised discovery of image classes (birds, cats, monitors)
- high variety within classes (types of dogs, viewpoints)
- even works with streetview images from Paris to discover architecture, scenes

Purity-coverage curve is SOTA / close-to SOTA
- 


### relative prediction pretext task
Testing the self-supervision task on Pascal
- 500 images from Pascal, 256 pairs from each
- 38.4% accuracy on the self-supervision task (12.5% is random baseline)

Training accuracy (on imagenet) is better though
- 39.5% training set
- 40.5% validation set

Filtering Pascal images to image bounding boxes 
- 240px min, not truncated, occluded, or difficult 
- 39.2% accuracy!
- algorithm is sensitive to whole image, works well with objects

## citation

```
@inproceedings{doersch2015unsupervised,
	title="Unsupervised Visual Representation Learning by Context Prediction",
	author="Carl {Doersch} and Abhinav {Gupta} and Alexei A. {Efros}",
	booktitle="IEEE International Conference on Computer Vision (ICCV)",
	pages="1422--1430",
	notes="Sourced from Microsoft Academic - https://academic.microsoft.com/paper/343636949",
	year="2015"
}
```
