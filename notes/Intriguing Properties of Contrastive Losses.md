---
title: Intriguing Properties of Contrastive Losses
created: '2020-11-24T03:09:13.428Z'
modified: '2020-11-24T05:05:56.855Z'
---

# Intriguing Properties of Contrastive Losses

## overview

- proposes a generalized version of contrastive loss $L_{alignment} + L_{distribution}$
- constructs datasets with explicit and controllable features
- demonstrates that easy-to-learn shared features *suppress* learning other sets, not present in AEs

## generalized constrastive loss
Break InfoNCE into two losses
$$
\begin{aligned}
\tau * L^{NT-Xent}_{x_i,x_j} &= - \tau \log \frac{\exp (sim (z_i,z_j) / \tau)}{\sum_{k=1}^{2n} 1_{k \not = i} \exp (sim (z_i,z_k) / \tau)} \\
&= - sim (z_i,z_j) + \tau \log \sum_{k=1}^{2n} \log (sim (z_i,z_k) / \tau) \\
&= L_{alignment} + \lambda L_{distribution}
\end{aligned}
$$

$L_{alignment}$ makes representations of augmented views consistent
- represents $H(U|V)$ for mutual info view
$L_{distribution}$ matches hidden representation to uniform on hypersphere
- represents $H(U)$ for mutual info view
- can use sliced wasserstein distance to do match distribution instead
- can use different distributions (uniform hypercube, gaussian)

SimCLR experiments 
- different distribution prior don't make a difference w/ big enough projection head
- $\lambda$ and $\tau$ work similarly

## feature suppression

digit-on-imagenet dataset with *channel addition*
- overlays mnist digit on imagenet image 9 times

MNIST features can interfere with ImageNet features
- vary # of unique MNIST digits in data
- supervised performance is unchanged
- SSL performance degrades strongly with more unique digits
- SSL performance *on MNIST* strongly increases

randbit dataset with *channel concat*
- choose random integer $n$ and concat it as $\log_2 n$ channels after RGB
- represent integer as $\log_2$ bits binary bits
- don't apply image augmentation to it

extra bits can interfere with image features (MNIST, CIFAR, ImageNet)
- $\sim 15$ extra bits and SSL performance strongly degrades
- larger batch size, lower temp help slightly but model size doesn't
- autoencoder reconstruction doesn't suffer (as much)

why? ideas
- digit-imagenet: competing features divide the dataset giving each one a unique id?
- randbit: distribution matching loss saturates when MI is super high already?

## citation

```
@article{chen2020-contrastive71,
    author = {Ting Chen and Lala Li},
    title = {Intriguing Properties of Contrastive Losses},
    year = {2020},
    booktitle = {arXiv preprint arXiv:2011.02803}
}
```
