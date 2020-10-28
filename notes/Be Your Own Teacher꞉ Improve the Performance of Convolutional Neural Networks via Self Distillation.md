---
attachments: [Clipboard_2020-10-15-17-39-07.png]
tags: [self-supervised, vision]
title: 'Be Your Own Teacher: Improve the Performance of Convolutional Neural Networks via Self Distillation'
created: '2020-10-14T21:58:01.415Z'
modified: '2020-10-15T23:10:41.457Z'
---

# Be Your Own Teacher: Improve the Performance of Convolutional Neural Networks via Self Distillation

## overview

- self-distillation increases performance, reduces training time
- improves ResNet50 accuracy by 2% 
- faster to train a high-accuracy reduced-size model

![](@attachment/Clipboard_2020-10-15-17-39-07.png)

## self-distillation

self-distillation
1. divide network into section (4 ResBlock in ResNet50)
2. add bottleneck (depth-wise conv) + classifier $q^i$ for each section
3. train using losses and teacher-student between deepest layer $C$ and shallow layers

losses
- cross-entropy on labels $(1 - \alpha) * CE(q^i,y)$
- KL between deepest layer and shallow layers (teacher-student distillation) $\alpha * KL(q^i,q^C)$
- L2 between feature maps of deepest and shallow layers (teacher-student ?) $\lambda * ||F_i - F_C||^2_2$


## experiments

CIFAR100 and ImageNet
- earlier classifiers (1/2 network size) can still do well on CIFAR100
- self-distillation boosts vanilla ResNet by 1-2% on ImageNet
- earlier layers are not as good on ImageNet

As performant as other distillation methods
- better than knowledge distillation but not significantly than AT or FitNet
- arguably faster since you don't have to train a model and then distill

Ever so mildly better than Deeply Supervised Nets
- it feels like there are far too many tricks for minimal final improvement

Does better when trained with gaussian noise
- they claim this means flat minima but I am super doubtful

Their vanishing gradient experiment shows that their model's gradients actually vanish faster???

## follow-up

ADENDA: self-distillation improves Hilbert space
- self-distillation sparsifies, reduces the number of basis in representation
- shows that there is collapse after some number of rounds


## citation

```
@inproceedings{zhang2019-convolutional15,
    author = {Linfeng Zhang and Jiebo Song and Anni Gao and Jingwei Chen and Chenglong Bao and Kaisheng Ma},
    title = {Be Your Own Teacher: Improve the Performance of Convolutional Neural Networks via Self Distillation},
    year = {2019},
    booktitle = {2019 IEEE/CVF International Conference on Computer Vision (ICCV)},
    pages = {3712-3721},
    publisher = {IEEE}
}
```
