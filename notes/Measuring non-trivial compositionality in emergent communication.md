---
attachments: [Clipboard_2020-11-03-15-10-23.png, Clipboard_2020-11-04-15-55-47.png, Clipboard_2020-11-04-15-56-42.png]
title: Measuring non-trivial compositionality in emergent communication
created: '2020-11-03T19:27:16.958Z'
modified: '2020-11-04T20:57:11.585Z'
---

# Measuring non-trivial compositionality in emergent communication

## overview

- measure non-trivial C using all previous metrics 
- only TRE detects NTC to a signficant degree
- correctly parametrized TRE separates c from non-c

![](@attachment/Clipboard_2020-11-03-15-10-23.png)

## intro

derivation is tree structure of input (symbolic view)
- d0 is base of the tree, concept

representation is message, 
- theta0 is base, symbol

trivial c has set intersect as c function
non-trivial c is function is more complex

TRE 
- using distance between representations
- learn compositional approx of your rep function f

## setup

implement c function as learned linear transform $s_1 \cdot s_2 \to As_1 + B s_2$

compositionality metrics
- generalization 80-20 random
- position disentanglement (from [[Compositionality and Generalization In Emergent Languages]])
- bag-of-words disentanglement (from [[Compositionality and Generalization In Emergent Languages]])
- [TRE](@note/Measuring Compositionality in Representation Learning) 
- context-independence (from Bogin et al 2018)
- topographic similarity
- conflict count (from Kucinski et al 2020)

nine protocols
- 1 trivial c
- 6 non-trivial c
  - entangled
  - diagonal
  - negation
  - rotated
  - context-sensitive
- 2 non c
  - random 
  - holistic

## experiments

only TRE accurately separates non-c from c (and include non-trivial)

![](@attachment/Clipboard_2020-11-04-15-55-47.png)

it's import that c function is correctly expressive
- using non-linear deep net means everything is seen as c

![](@attachment/Clipboard_2020-11-04-15-56-42.png)


## citation

```
@misc{korbak2020measuring,
      title={Measuring non-trivial compositionality in emergent communication}, 
      author={Tomasz Korbak and Julian Zubek and Joanna Rączaszek-Leonardi},
      year={2020},
      eprint={2010.15058},
      archivePrefix={arXiv},
      primaryClass={cs.NE}
}
```
