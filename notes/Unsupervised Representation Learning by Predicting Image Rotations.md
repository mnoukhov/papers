---
attachments: [Clipboard_2020-09-10-18-22-46.png]
tags: [self-supervised, vision]
title: Unsupervised Representation Learning by Predicting Image Rotations
created: '2020-09-10T15:43:25.688Z'
modified: '2020-09-10T23:31:19.036Z'
---

# Unsupervised Representation Learning by Predicting Image Rotations

## overview

- predict image rotation $[0, 90, 180, 270]$ as SSL task
- achieves unsupervised SOTA on CIFAR-10, Imagenet, Places, PASCAL
- narrows gap b/w supervised and SSL

![](@attachment/Clipboard_2020-09-10-18-22-46.png)

## method
RotNet
1. generate 4 rotations for every image $\theta in [0, 90, 180, 270]$ 
2. _classify_ rotation $\theta$ given an image

reasoning 
- as opposed to [[Discriminative Unsupervised Feature Learning with Exemplar Convolutional Neural Networks]] it isn't learning invariance but rather explicitly learning right side up
- 90 degree rotations can be implemented with flips and without image artifacts
- it is a strong semantic signal to understand whether an image is "right side up"

notes
- better performance feeding all 4 rotations in each batch (batch size * 4)

## results

CIFAR-10 unsupervised SOTA with SSL + 3-layer MLP finetune
- larger model leads to better performance
- supervised only slightly outperforms SSL + finetune
- but SSL + finetune converges faster and with fewer examples than supervised
  
2nd conv block has the best features
- later layers are very rotation-specific as shown by attn maps
- lower conv block attn are more similar to supervised

4 rotations is optimal
- underperformance with 2
- no increase with 8

SOTA on other tasks
- Imagenet classification
- Pascal VOC transfer learning
- 205-way Places classification

## discussion 
keeping $0$ rotation is likely necessary for test images
- having the original images might be very useful 

why is it helpful having all 4 rotations of an image in each batch 
- reducing variance in SGD update?
- old school MNIST used to have one of each digit in each batch

why should we expect supervised NIN to be better than unsupervised + finetune
- they are both trained to convergence, does that mean SSL features are bad?
- could be artifact of NIN networks, not present in newer skip networks

Rotation Feature Decoupling 
- try to get rotation-invariant features that will be more useful downstream
- RotNet classification + Rotation Irrelevance + Instance classification


## citation
```
@inproceedings{gidaris2018-representation60,
    author = {Spyros Gidaris, Praveer Singh and Nikos Komodakis},
    title = {Unsupervised Representation Learning by Predicting Image Rotations},
    year = {2018},
    booktitle = {International Conference on Learning Representations (ICLR)}
}
```


