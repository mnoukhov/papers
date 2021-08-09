---
attachments: [Clipboard_2021-05-18-00-54-12.png, Clipboard_2021-06-02-15-26-33.png, Clipboard_2021-06-09-15-31-37.png, Clipboard_2021-08-09-00-01-00.png, Clipboard_2021-08-09-00-19-38.png, Clipboard_2021-08-09-00-20-21.png]
title: Learning with Latent Language
created: '2021-05-18T03:51:13.886Z'
modified: '2021-08-09T06:57:26.846Z'
---

# Learning with Latent Language

## overview

- use language as a latent parameter space for few-shot and RL
- L3
  - pretrain language+input model $f$ (e.g. image-text classification)
  - learn input to language model $q$ (e.g. image captioning)
  - sample text from $q$ to optimize $f$ for new inputs 
- language as a latent space encourages compositional generalization!

## learning with latent language

![](@attachment/Clipboard_2021-06-09-15-31-37.png)

$f$: language pretraining
- learn language interpretation model $f(x;\eta, w)$ for image $x$, text $w$, weights $\eta$ 
- e.g. image rating model: how well does image $x$ match description $w$  

$q$: concept-learning pretraining
- learn proposal model $q$ that finds optimal text $w$ for each image $x$
- essentially find best language string that minimizes language interpretation loss
- e.g. image captioning model: given image $x$, output description $w$

evaluation
- use language string to rep new images using $q$ and $f$
- use proposal $q$ to get $n$ description samples for some image $x$
- choose description $w$ that minimizes image rating $f$ loss


![](@attachment/Clipboard_2021-06-02-15-26-33.png)


## few shot classification
![](@attachment/Clipboard_2021-05-18-00-54-12.png)

shapeworld pattern task
- given four images, all examples of some concept (e.g. blue shape near yellow triangle)
- decide yes/no whether fifth image matches the concept
- captions generated with a grammar from the DMRS representation

model
- input image feats $\textrm{rep}(x)$ are last layer VGGNet + 2 traininable FC w/ ReLU 
- image classification $f = \sigma(\textrm{rnn-enc}(w)^T \textrm{rep}(x))$ is similarity b/w word and image rep
- image captioning $q$ uses average of 4 images hidden reps as input to generate true text $q(w | x_j) = \textrm{rnn-dec}(w|\frac{1}{n}\sum_j \textrm{rep}(x_j))$

baselines
- multitask 
  - $f = \sigma(\theta_i^T rep(x))$ for task specific $\theta_i$
  - predict class based just off of image rep, and $\theta_i$ learned on four examples
  - learn task-specific $\theta_i$ but task-agnostic $\textrm{rep}(x)$ 
- meta learning 
  - $f = \sigma([\frac{1}{n}\sum_j rep(x_j)]^T rep(x))$
  - similarity of $\textrm{rep}(x)$ between four class images and new input image
  - learning a good image representation that encapsulates the tasks
- meta learning + joint adds predicting $q$ during pretraining
  - predict language string $w$ using average image $\textrm{rep}$
  - seems exactly like LSL, but didn't end up working (whereas LSL does)

results
- L3 reasonably outperforms baselines 
- L3 can make correct final outputs even with incorrect generated captions $w$ (e.g. "circle left of square" when all you need is just "circle")

## programming by demonstration

![](@attachment/Clipboard_2021-08-09-00-01-00.png)

string editing task
- five examples of string transformation are given
- apply appropriate transformation to an input string
- new dataset
  - sample regular transducers
  - apply to random dictionary words
  - get mTurk to describe them in NL

model
- rep is RNN encode concat 
$\textrm{rep}(x,y) = \textrm{rnn-encode}([x,y])$
- predict by decoding from a concat of input string and text encoded 
$f(y | x ; w) = \textrm{rnn-decode}(y | \textrm{rnn-encode}(x),\textrm{rnn-encode}(w))$
- predict string by decoding from average input rep 
$q(w | \lbrace x_i,y_i \rbrace ) = \textrm{rnn-decode}(w | \frac{1}{n} sum_i \textrm{rep}(x,y))$
- same baselines

results
- L3 strongly outperforms baselines
- using regex as the latent space isn't as good as NL
  - but ground truth regex is best 
  - using a regex exeuction engine ($\textrm{eval}$ instead of $f$) is slightly better 
- best NL inference (max $q$ over 100 samples) is slightly better than ground truth
  - may be due to bad ground truth annotations by mturk


## policy search

![](@attachment/Clipboard_2021-08-09-00-20-21.png)

treasure hunting task
- there is a hidden treasure at some point on the map
- it is consistently next to some landmark
- the agent must nagivate to it and perform DIG action
- the goal is adaptation in fewer episodes vs random policy search
- natural language ground truth from original dataset

model
- rep is 2-layer FC + tanh over image
- instruction following $f$ 
$f(a | x;w) \propto \textrm{rnn-encode}(w)^T \ W_a \ \textrm{rep}(x)$
- instruction-generating $q$
$q(w) = \textrm{rnn-decode}(w)$
- multitask baseline uses task embeddings instead of NL

training
- pretrain language 
  - given NL description $w$ and expert policy to get to the treasure
  - train using DAgger to get good $w$ generation
- start training: generate $w$ and estimate values
  - concept learning phase
  - sample multiple descriptions $w$
  - for each $w$ do multiple rollouts 
  - take average reward to get value of $w$
- continue training: finetune highest scoring $w$ 
  - vanilla PG finetune highest scoring $w$ 

results
- L3 outperforms multitask and strongly for scratch
- not perfect performance so still some overfitting
- NL as a representation space for policies

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
