---
title: 'Rethinking Few-Shot Image Classification: a Good Embedding Is All You Need?'
created: '2020-11-24T05:06:40.649Z'
modified: '2020-11-27T19:36:26.381Z'
---

# Rethinking Few-Shot Image Classification: a Good Embedding Is All You Need?

## overview

- self-supervised + linear finetune gets SOTA on meta-learning 
- self-distillation improves performance, 7\% over SOTA on Meta-Dataset
- good self-supervised matches supervised 

##  SSL baseline

supervised transfer as meta-learning
- merge $N$ meta-trains of $K$-way classification into one dataset
- train image classifier on $NK$-way classification
- freeze network as embeddings
- linear finetune + eval on meta-test  

sequential self-distillation
- same size model minimize KL between predictions and soft teacher targets
- born-again: self-distill many generations

training
- model is ResNet-12 + DropBlock
- data augment: random crop, color jitter, horizontal flip

datasets
- miniImageNet
- tiered ImageNet
- CIFAR-FS
- FC100
- Meta-Dataset

## experiments

supervised + finetune beats meta-learning on ImageNet,CIFAR
- cross-entropy train, linear eval beats fancier other baselines
- self-distill does even better, clear SOTA

SSL + finetune is nearly as good
- using CMC or MoCo on meta-train

ablations
- linear regression is better than nearest neighbour with $L_2$
- cosine sim (nearest with $L_2$ norm) can be as good as linear regression
- $L_2$ normalization of embeddings space helps

improvements
- data augmentation of meta-test
- sequential distillation but usually only up to 3rd gen
- more data + bigger networks 

SOTA on Meta-Dataset
- ResNet-18, 2x self-distill
- logistic + distill is SOTA on 9/10 test-tasks



## citation

```
@article{tian2020-classification90,
    author = {Yonglong Tian and Yue Wang and Dilip Krishnan and Joshua B. Tenenbaum and Phillip Isola},
    title = {Rethinking Few-Shot Image Classification: a Good Embedding Is All You Need?},
    year = {2020},
    booktitle = {arXiv}
}
```
