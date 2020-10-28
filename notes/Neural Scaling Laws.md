---
attachments: [Clipboard_2020-10-26-12-56-55.png, Clipboard_2020-10-26-13-14-35.png, Clipboard_2020-10-26-13-16-26.png, Clipboard_2020-10-26-23-04-08.png, Clipboard_2020-10-26-23-05-35.png, Clipboard_2020-10-26-23-07-04.png]
title: Neural Scaling Laws
created: '2020-10-22T18:05:18.067Z'
modified: '2020-10-27T03:08:26.336Z'
---

# Neural Scaling Laws

## overview 
Language model performance scales as a power-law with
- model size
- dataset size
- amount of compute used for training
Other architectural features seem to have minimal impact over large scales

Calculating optimal training per compute we want very large models trained on a modest amount of data stopped very shot of convergence

## summary

__smooth power laws__ between number of params $N$, dataset size $D$, and compute $C$ and test loss on language modelling
- need to scale $N$ and $D$ together in 8x, 5x ratio 
- convergence is inefficient and 
- can predict training given early part

__performance depends on scale__ more than architecture but depth / width ratio is important too

__transfer improves with test performance__

__larger models are more sample efficient__

## Setup

WebText2 96GB
- 3 Karma+ Reddit posts (like WebText)
- WebText 1 (Dec 2017) + posts from Jan - Oct 2018

Ranges
- model params $N: 768 \to 1.5B$
- dataset size $D: 22M \to 23B$ tokens
- shape (depth, width, attention-heads, ff dim)
- context length = 1024
- batch size usually $2^19$


## Scaling Laws

Test loss power-laws scales with $C, N, D$
![](@attachment/Clipboard_2020-10-26-12-56-55.png)

- loss for a model trained to convergence $L(N) = (N_c / N)^{alpha_N}, \alpha_N \sim 0.076$
- large model trained with a limited dataset $L(D) = (D_c/D)^{\alpha_D}, \alpha_D \sim 0.095$
- using limited compute, enough data, optimal model $L(C_{min}) = (C_c^{min}/C_{min})^{\alpha_C^{min}}, \alpha_C^min \sim 0.050$

Bigger models are more efficient given a compute budget and goal test loss
![](@attachment/Clipboard_2020-10-26-13-16-26.png)

Show how most scaling is in model size, a little bit in data which mostly goes towards bigger batch sizes 
![](@attachment/Clipboard_2020-10-26-13-14-35.png)

### Factors in Scaling

Other factors (depth, shape, ff dim) are not as important holding $N$ constant
![](@attachment/Clipboard_2020-10-26-23-04-08.png)

Similar for number of layers (> 2)
![](@attachment/Clipboard_2020-10-26-23-05-35.png)

Transformers do seem to scale better than LSTMs likely because of long-range dependencies
![](@attachment/Clipboard_2020-10-26-23-07-04.png)

These effects hold on other datasets

## Infinite Data Limit and Overfitting



## citation

```
@article{kaplan2020-neuralscalinglaws,
    author = {Jared Kaplan and Sam McCandlish and Tom Henighan and Tom B. Brown and Benjamin Chess and Rewon Child and Scott Gray and Alec Radford and Jeffrey Wu and Dario Amodei},
    title = {Scaling Laws for Neural Language Models},
    year = {2020},
    booktitle = {arXiv preprint arXiv:2001.08361}
}
```

