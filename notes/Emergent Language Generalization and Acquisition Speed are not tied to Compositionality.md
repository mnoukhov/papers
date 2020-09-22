---
tags: [emergent]
title: Emergent Language Generalization and Acquisition Speed are not tied to Compositionality
created: '2020-09-21T19:35:48.815Z'
modified: '2020-09-21T20:03:45.793Z'
---

# Emergent Language Generalization and Acquisition Speed are not tied to Compositionality

## overview

- compositional EC languages are not necessary for generalization
- depending on task, compo

## intro

compositionality (C)
- meaning of an expression is a function of meaning of parts
- EC language is C if inputs are disentangled
- each symbol should refer to **only** one input
- *naive C*: no symbol interdependence

sender-receiver game
- hard-coded sender controls C of game
- LSTM/GRU agents
- 

## attribute-value game (*attval*)

input 2 attributes of size $n_v$ each, length-2 message
- *lang-identity* encodes as-is
- *lang-entangled* $m_j \gets (i_1 + (-1)^j \cdot i_2) \mod n_v$

3 tasks for each language
- *identity* reconstruct input
- *linear* $A \cdot i + b \mod n_v$
- *entangled* $(i_1 + (-1)^j \cdot i_2) \mod n_v$

each language works best for its task
- faster acquisition speed and generalization for your own task
- worse on the opposite task
- equal performance on *linear*

## coordinate game 

sample input from unit circle, length-2 message
- *lang-coordinate* gives X, Y coordinates
- *lang-rotated* $\pi/4$ rotated coords





## citation

```
@article{kharitonov2020-compositionality1,
    author = {Eugene Kharitonov and Marco Baroni},
    title = {Emergent Language Generalization and Acquisition Speed are not tied to Compositionality.},
    year = {2020},
    booktitle = {arXiv preprint arXiv:2004.03420}
}
```
