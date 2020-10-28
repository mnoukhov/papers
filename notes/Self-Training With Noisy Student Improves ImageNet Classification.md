---
attachments: [Clipboard_2020-10-18-13-27-45.png]
tags: [self-supervised, vision]
title: Self-Training With Noisy Student Improves ImageNet Classification
created: '2020-10-08T22:44:18.024Z'
modified: '2020-10-18T18:07:55.208Z'
---

# Self-Training With Noisy Student Improves ImageNet Classification

## overview

- iterative self-training where student has noise in input 
- SOTA on ImageNet, 88.4, semi-supervised, using EfficientNet-L2
- SOTA on ImageNet robustness (A,C,P) by large margin 


![](@attachment/Clipboard_2020-10-18-13-27-45.png)


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

## setup

labelled ImageNet classification
unlabelled JFT dataset 
- use ImageNet-trained EfficientNet-B4 to find images with label confidence > 0.3
- for each class select 130k most confident images
- duplicate images if not enough 
- 130M images total, 81M unique

models
- EfficientNet-B7, 2x width, 66M params
- EfficientNet-L2, 4.3x width, 480M params

training
- batch together labelled and unlabelled images
- fix test-train resolution diff by finetuning with larger resolution for 1.5 epochs on unaugmented labeled images


iterative training

0. ENet-B7 teacher trained on ImageNet
1. ENet-L2 student trained on ImageNet + 14x teacher-labelled JFT. Becomes teacher
2. ENet-L2 trained on ImageNet + 14x teacher-labelled JFT. Becomes teacher.
3. final ENet-L2 trained on ImageNet + 28x teacher-labelled JFT

## results

SOTA on ImageNet (semi-supervised)
- Noisy Student makes ENet-L2 go $85.5\% \to 88.4\%$ 
- twice as small as previous SOTA 

Noisy student w/o iterative training also works
- EfficientNets-B[0,7] get average $0.8\%$ improvement

Large SOTA on robustness ImageNet-A,C,P
- A accuracy $61\% \to 83.7\%$
- C mean corruption error $45.7\% \to 28.3\%$
- P mean prob of flipping prediction under perturbation $23.7 \to 12.2$

More robust to adversarial attack
- less drop-off compared to regular EfficientNet-L2

## ablations

noisy student leads to improvements
- outperforms student w/ less noise or teacher w/ noise

iterative training leads to logarithmic improvements
- $87.6 \to 88.1 \to 88.4$

extra appendix ablations 
- large teacher model 
- large amount of unlabelled data is necessary
- larger student model is important
- joint training on labelled + unlabelled outperforms first one then the other
- large ratio of unlabelled to labeled batch size

other extra appendix ablations
- soft pseudo-labels can be better than hard of OOD data
- data balancing for small models
- training student from scratch can be better!! (in contrast to [SIL](@note/Countering Language Drift with Seeded Iterated Learning))



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
