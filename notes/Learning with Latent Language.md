---
attachments: [Clipboard_2021-05-18-00-54-12.png, Clipboard_2021-06-02-15-26-33.png, Clipboard_2021-06-09-15-31-37.png]
title: Learning with Latent Language
created: '2021-05-18T03:51:13.886Z'
modified: '2021-06-15T20:51:50.943Z'
---

# Learning with Latent Language

## overview

- use language as a latent parameter space for few-shot learning
- L3
  - pretrain by language learning (image captioning)
  - sample $n$ captions for new images
  - use the best caption extra latent representation


## learning with latent language

![](@attachment/Clipboard_2021-06-09-15-31-37.png)

training $f$: pretraining
- learn language interpretation model $f(x;\eta, w)$ for image $x$, text $w$, weights $\eta$ 
- e.g. image rating model: how well does image $x$ match description $w$  
- learn proposal model $q$ to generate $w$ for $x$

training $q$: concept-learning
- learn proposal model $q$ that finds optimal $w$ for each $x$
- essentially find best language string that minimizes language interpretation loss
- e.g. image captioning model: given image $x$, output description $w$

evaluation
- use language string to rep new images using $q$ and $f$
- use proposal $q$ to get $n$ description samples for some image $x$
- choose description $w$ that minimizes image rating $f$ loss
- 


![](@attachment/Clipboard_2021-06-02-15-26-33.png)


## few shot classification
Shapeworld pattern task
- given four images, all examples of some concept (e.g. blue shape near yellow triangle)
- decide yes/no whether fifth image matches the concept

![](@attachment/Clipboard_2021-05-18-00-54-12.png)

Model
- image feats $rep(x)$ are last layear VGGNet + 2 traininable FC + ReLU 
- image classification $f = \sigma(\textrm{rnn-enc}(w)^T rep(x))$
- learn image captioning with average of 4 images hidden reps as input $q(w | x_j) = \textrm{rnn-dec}(w|\frac{1}{n}\sum_j rep(x_j))$

baselines
- multitask $f = \sigma(\theta_i^T rep(x))$ for task specific $\theta_i$
- meta learning $f = \sigma([\frac{1}{n}\sum_j rep(x_j)]^T rep(x))$
- meta learning + joint adds predicting $q$ during pretraining


## citation

```
@inproceedings{andreas2018-learning78,
    author = {Jacob Andreas and Dan Klein and Sergey Levine},
    title = {Learning with Latent Language},
    year = {2018},
    booktitle = {Proceedings of the 2018 Conference of the North American Chapter of the
      Association for Computational Linguistics: Human Language Technologies,
      Volume 1 (Long Papers)},
    pages = {2166-2179},
    publisher = {Association for Computational Linguistics},
    volume = {1}
}
```
