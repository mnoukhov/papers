---
title: Incorporating Pragmatic Reasoning Communication into Emergent Language
created: '2020-12-09T02:50:04.913Z'
modified: '2020-12-09T17:41:56.090Z'
---

# Incorporating Pragmatic Reasoning Communication into Emergent Language

## overview

- adding ``pragmatism'' to EC with framework
- improves referential game performance and Starcraft II sim

## pragmatic models

referential game
- ``long term'' is regular RL used as pretraining ?

one-sided pragmatics: speaker models listener $\argmax_m P_S(m|c)^\lambda P_L(c|m)^\lambda $
- SampleL has listener sample $P_L(m)$ 
- ArgmaxL has listener choose $\argmax P_L(m)$

two-sided pragmatics: update speaker and listener models for each possible object $t$
- RSA $P_{S_{k+1}} \propto P_{L_k}(t|m) P_{S_k}(m|t)$ and $P_{S_{k+1}} \propto P_{L_k}(t|m) P_{S_k}(m|t)$
- Iterated Best Response $P_{S_{k+1}}(m|t) = \delta[m - \argmax_m P_{L_k}(t|m) P_{S_k}(t|m)]$ and $P_{L_{k+1}}(t|m) = \delta[m - \argmax_t P_{S_{k+1}}(m|t)]$

game theoretic pragmatics
- enumerate all possible inputs, messages, outputs 
- give reward $P(m|t),P(t|m)$ if success, $0,0$ otherwise
- 

## issues

pragmatics can only occur if there are more than the learning agents to begin with, otherwise pragmatics has no meaning!


### related work

Cao et al is cited as a paper on pragmatics, it is not

emergent languages DO NOT show characeristics of human languages
- semantic drift is a non-human issue!
- only trivial compositionality has been found

LOLA is not cited for opponent modelling

### pragmatics

if you are doing argmax over $P_L$ (or even have access to it) why not just do $\argmax_m P_L(c|m)$?

speakers strategy is $C \times M_U$

## citation

```
@inproceedings{kang2020-incorporating20,
    author = {Yipeng Kang and Tonghan Wang and Gerard de Melo},
    title = {Incorporating Pragmatic Reasoning Communication into Emergent Language},
    year = {2020},
    booktitle = {Advances in Neural Information Processing Systems},
    volume = {33}
}
```
