---
attachments: [Clipboard_2020-10-14-11-24-42.png]
tags: [self-supervised]
title: When Does Label Smoothing Help?
created: '2020-10-14T14:56:49.844Z'
modified: '2020-10-14T16:33:16.994Z'
---

# When Does Label Smoothing Help?

## overview

- new visualization method on penultimate layer of network
- show that label smoothing calibrates models to align prediction confidence with accuracy
- label smoothing impairs distillation

## label smoothing

label smoothing (LS) compute cross entropy with weighted mixture of true target and uniform distribution

$$
H(y,p) = \sum_{k=1}^K -(y_k(1-\alpha) + \alpha/K) log(p_k)
$$

LS makes the difference between correct logits and incorrect logits to be a constant dependent on $\alpha$
- regular training allows incorrect logits to be wildly different

LS encourages penultimate $x^T$ to be close to pre-softmax weights $w_k$
- pre-softmax $x^T w_k$ can be considered a measure of $L_2$ distance between $x^T$ and $w_k$

## penultimate layer representation

1. pick three classes $i,j,k$
2. find an orthonormal basis for the plane for $w_{i,j,k}$
3. project $x$ with classes $i,j,k$ to this plane

![](@attachment/Clipboard_2020-10-14-11-24-42.png)

with LS
- tighter penultimate activation clusters
- equidistant activations (triangle shape)
- lower magnitude (lower confidence?)
- erased degree of change between clusters (?) 

## model calibration

neural networks are overconfident
- measure accuracy vs confidence with ECE
- temperature scaling softmax can improve this

LS calibrates implicitly
- slightly surprising as training examples are collapsed to tight ball, validation examples do spread (range of confidence)

LS helps beam search for machine translation
- model confidence is important for next-token selection
- minimum ECE is predictive of best BLEU score
- LS does better than temperature scaling can
- NLL is worse (because we're smoothing, obvs)


## model distillation

knowledge distillation trains student $p$ to approximate soft teacher $p^t$, replace CE with weighted sum and scale with temp $T$
$$
H(y,p) = (1-\beta)H(y,p) + \beta H(p^t(T),p(T))
$$

CIFAR 10
- ResNet 56 teacher
- AlexNet student 
- distillation w/ temp scaling teacher or LS teacher

label smoothing can improve teacher network, but students are worse
- because of tight clustering, each example is not too different from one-hot encoding
- erases information about sample variance within class (LS reduces the MI between clusters) 

## citation

```
@inproceedings{müller2019when,
	title="When does label smoothing help",
	author="Rafael {Müller} and Simon {Kornblith} and Geoffrey E {Hinton}",
	booktitle="NeurIPS 2019 : Thirty-third Conference on Neural Information Processing Systems",
	pages="4694--4703",
	year="2019"
}
```
