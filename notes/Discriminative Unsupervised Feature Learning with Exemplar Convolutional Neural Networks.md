---
tags: [self-supervised, vision]
title: Discriminative Unsupervised Feature Learning with Exemplar Convolutional Neural Networks
created: '2020-09-08T14:45:55.687Z'
modified: '2020-09-10T22:17:48.037Z'
---

# Discriminative Unsupervised Feature Learning with Exemplar Convolutional Neural Networks

## paper

### overview

- learning un/self-supervised representation for images
- transformation from seed, Exemplar-CNN
- unsupervised SOTA on STL10, CIFAR10, Caltech
- outperforms Alexnet, SIFT on descriptor matching 

### surrogate data

1. sample $N$ patches of images
  32x32 pixels with ``considerable gradient'' to get objects
  sample patch proportional to its mean-squared gradient magnitude
  *note*: gradient magnitude is a basic edge filter
2. decide on transformations
  translation, scaling, rotation, PCA contrast, saturation, hue shift
3. sample $K$ transformation vectors for each patch
4. subtract mean of pixels over whole dataset

### exemplar-CNN

consider each patch $N$ to be a class and learn a CNN to discriminate between them

proof that the global minimum is when learned representation is invariant to transformations

discriminative task (in contrast to generative) leaves more freedom to model individual samples

### classification

#### setup

surrogate train data from unlabeled 100k from STL-10

three archs
- small 3 layer CNN for probing
- medium 4 layer CNN compare to SOTA
- wider 4 layer CNN compare to SOTA

1 vs all SVM trained on final pooled features 

#### results

SOTA on all unsupervised tasks (STL-10, CIFAR-10, Caltech)

Outperforming supervised SOTA on STL-10 which has fewer labelled examples

Performance increases with $N$ (surrogate data classes) until a certain point
- if $N$ is too big, overlapping samples make the problem hard and performance worse
- clustering can allow grouping similar classes

Performance increases with $K$ (number of samples per class) until about 100

Transformations are not of equal important
- rotation and scaling improve minimally (CNN bias?)
- translation, color, and contract are useful (especially on STL-10 and Caltech)

Learned features
- are relatively dataset agnostic
- have better invariance to transformations

Learning algorithm scales well with architecture

### descriptor matching 

#### setup

synthetic matching dataset
- 16 images from flickr 
- geometric transforms (rotation, zoom, perspective, non-linear)
- non-geometric transforms (lighting, blur)
- matched 24 tranforms to each image, 384 pairs

matching task
- extract ellipse from image using MSER
- extract patch from ellipse 
- get descriptor of patch using model
- get pair by finding closest descriptor from Euclidean distance
- true positive if IOU of ellipses is > 0.5

compared Exemplar-CNN features to SIFT, AlexNet using AUC of precision-recall

optimal patch size used for each model

#### results

Exemplar-CNN and Alexnet generally outperform SIFT
- but do significantly worse on blurred images

Exemplar-CNN trained with blur nearly fully outperforms SIFT


## discussion

### questions

might not be linearly scalable 

using a linear classifier for evaluation?
- MLPs might not improve greatly (SimCLR)

- assumption that gradient in patches means containing objects
- what is the PCA contrast?

### follow-up

Multi-task Self-supervised Learning (ICLR 2017) used Exemplar-CNN and made improvements
- triplet loss instead of softmax
- resnet for encoder
- training on imagenet

They found that performance on imagenet does not always increase 
- possible explanation is that it overfits to the transformations you specify

Non-Parametric Instance Discrimination (CVPR 2018) directly improves on it
- non-parametric softmax (NPS) better than softmax
- NCE approx NPS, can scale to be as good
- memory bank caches most recent features 

Exemplar CNN and Information Maximization (2017)
- Exemplar CNN is actually optimizes the variational lower bound of a batch and its representation 
- lower bound isn't necessarily tight
- using MLP may improve infomax but linear layer may improve features' linear separability


## citation

```
@incollection{NIPS2014_5548,
  title = {Discriminative Unsupervised Feature Learning with Convolutional Neural Networks},
  author = {Dosovitskiy, Alexey and Springenberg, Jost Tobias and Riedmiller, Martin and Brox, Thomas},
  booktitle = {Advances in Neural Information Processing Systems},
  editor = {Z. Ghahramani and M. Welling and C. Cortes and N. D. Lawrence and K. Q. Weinberger},
  pages = {766--774},
  year = {2014},
  publisher = {Curran Associates, Inc.},
  url = {http://papers.nips.cc/paper/5548-discriminative-unsupervised-feature-learning-with-convolutional-neural-networks.pdf}
}
```
