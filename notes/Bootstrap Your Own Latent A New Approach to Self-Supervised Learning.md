---
attachments: [Clipboard_2020-10-08-18-05-58.png]
tags: [self-supervised]
title: Bootstrap Your Own Latent A New Approach to Self-Supervised Learning
created: '2020-10-08T21:59:51.112Z'
modified: '2020-10-08T23:22:37.768Z'
---

# Bootstrap Your Own Latent A New Approach to Self-Supervised Learning

## overview

- BYOL gets unsupervised SOTA on imagenet without negative samples
- predict target network view from online network view as SSL task
- SOTA on semi-supervised and transfer learning 
- BYOL shown more resilient to different image augmentations

## BYOL

![](@attachment/Clipboard_2020-10-08-18-05-58.png)

online network $\theta$ tries to predict target network $\xi$
target network is exp moving average $\xi \gets \tau \xi + (1-\tau)\theta$

loss is MSE between $L_2$ normalized prediction and target
- $L = ||\bar q_\theta(z_\theta) - \bar z'_\xi||_2^2$
- stop-gradient (sg) on target since we update it with moving average

keep $f$ as final encoder for transfer

architecture
- encoder $f$ is ResNet-50 or modified
- projector $g$ is MLP (4096, BN, ReLU, 256) 
- predictor $q$ same as $g$

same image augmentations as [SimCLR](@note/A Simple Framework for Contrastive Learning of Visual Representations)


$\lambda^l$

## experiments

SOTA on SSL, beating SimCLR, MoCo, CMC
- linear evaluation gets SOTA at every ResNet size
- semi-supervised gets SOTA for 1%, 10%, top1, top5 

SOTA on transfer 
- best or comparable on (Food101, CIFAR10,100,  Birdsnap, Caltech...) for linear and finetune
- best on semantic segmentation, object detection (VOC) and depth estimation (NYUv2)

Ablations
- does better with smaller batch compared to SimCLR
- does better with fewer augmentations compared to SimCLR
- decay rate $\tau$ between 0.9 and 0.999 is optimal

Connection to InfoNCE
- extend infoNCE to bigger objective 

## discussion
Why doesn't it collapse, UntitledAI blog post
- predictor MLP is crucial and asymmetry gives a specific extra *cross-model* term ("Run Away From Your Teacher")
- BN is also essential, no BN or LayerNorm doesn't work
- maybe BN means this is implicit contrastive of this image vs *average image*

LARS optimizer
- layer-wise adaptive learning rate
- learning rate increase if weights are large or gradients are small
- collapsing representation would likely quickly change the learning rate 

connection to iterative/self-training?


## citation

```
@article{collab2020-supervised59,
    author = {Jean-Bastien Grill, Florian Strub, Florent Altche, Corentin Tallec, Pierre H. Richemond, Elena Buchatskaya, Carl Doersch, Bernardo Avila Pires, Zhaohan Daniel Guo, Mohammad Gheshlaghi Azar, Bilal Piot, Koray Kavukcuoglu, Remi Munos and Michal Valko},
    title = {Bootstrap Your Own Latent: A New Approach to Self-Supervised Learning},
    year = {2020},
    booktitle = {arXiv preprint arXiv:2006.07733}
}
```



