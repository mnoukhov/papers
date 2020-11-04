---
tags: [emergent]
title: Emergent Language Generalization and Acquisition Speed are not tied to Compositionality
created: '2020-09-21T19:35:48.815Z'
modified: '2020-11-03T19:25:01.666Z'
---

# Emergent Language Generalization and Acquisition Speed are not tied to Compositionality

## overview

- compositional languages are only better for tasks that have the same composition
- measurements of compositionality should take task into account
- stop focusing on compositionality and focus on systematicity, productivity

## setup

compositionality (C)
- meaning of an expression is a function of meaning of parts
- EC language is C if inputs are disentangled
- *naive C*: each symbol refers to only one attribute, no symbol interdependence
  - in contrast Kottur et al find contextual language non-compositional
  - Havrilov and Titov use symbol-position combinations that encode one thing

one-step sender-receiver game
- hard-coded sender controls C of game
- LSTM/GRU agents

evaluate
- acquisition speed (epochs to get perf)
- generalization on randomly held out data (20%)

## attribute-value game (*attval*)

input 2 attributes of size $n_v$ each, length-2 message
- *lang-identity* encodes as-is
- *lang-entangled* $m_j \gets (i_1 + (-1)^j \cdot i_2) \mod n_v$

3 tasks for each language
- *identity* reconstruct input
- *linear* $A \cdot i + b \mod n_v$
- *entangled* $(i_1 + (-1)^j \cdot i_2) \mod n_v$

**result** each language works best for its task
- faster acquisition speed and generalization for your own task
- worse on the opposite task
- equal performance on *linear*

## coordinate game 

sample input from unit circle, length-2 message
- *lang-coordinate* gives X, Y coordinates
- *lang-rotated* $\pi/4$ rotated coords

reconstruct *non-rotated* input
- linear layer should be able to learn rotation easily

**result** learning curves are identical even though one representation is worse compositionally

## discussion

nothing special about compostionality *in isolation from the target task*
- the correct representation depends on the task
- we need to be clear which type of compositionality we care about


## citation

```
@article{kharitonov2020-compositionality1,
    author = {Eugene Kharitonov and Marco Baroni},
    title = {Emergent Language Generalization and Acquisition Speed are not tied to Compositionality.},
    year = {2020},
    booktitle = {arXiv preprint arXiv:2004.03420}
}
```
