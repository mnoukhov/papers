---
tags: [emergent]
title: Compositionality and Generalization In Emergent Languages
created: '2020-10-09T17:13:29.483Z'
modified: '2020-11-02T06:13:19.056Z'
---

# Compositionality and Generalization In Emergent Languages

## overview

- two new measures of compositionality for generalization
  - bag of symbols disentanglement
  - position disentanglement
- generalization happens when input space is sufficiently large 
- compositionality isn't necessary for generalization but sufficient
- compositionality is more easily learnable

## game

1-step symbolic sender-receiver game
- 2-4 inputs types
- 4-100 possible values per type
- sender vocab size $\in (5,10,50,100)$
- sender message length $\in (3,4,6,8)$

agents are 1-layer GRU
- REINFORCE sender, backprop receiver

## measuring compositionality

random 90-10 split generalization 
- for runs that converge with 99.99% train acc

topographic similarity (topsim)
- correlation between input distance and message distance
- doesn't measure whether correlation is compositionality

position disentaglement (posdis)
- whether symbols in specific positions always refer to the same attribute
- $\frac{1}{seq len} \sum_j \frac{I(s_j;a_1^j) - I(s_j;a_2^2)}{H(s_j)}$
- difference between highest MI of attribute to position $j$ ($a_1^j$) and second highest ($a_2^j$) 
- normalized with entropy of word distribution and averaged over all positions

bag-of-symbols disentanglement (bosdis)
- whether specific symbols always refer to the same attribute regardless of position
- $\frac{1}{vocab size} \sum_j \frac{I(n_j;a_1^j) - I(n_j;a_2^2)}{H(n_j)}$
- same except using count of vocab item $n_j$


## experiments

generalization as long as input space is sufficiently large 
- not about larger training size, just a value occuring with a large amount of other values
- test accuracy improves with larger input size, as long as message space is bigger
- $|C| > 5.9 |I|$ for train accuracy to be 100%

generalization does not require compositionality
- rarely is compositionality correlated to generalization
- but high compositionality usually implies generalization 
  posdis $> 0.5 \implies p(\text{generalization}) > 0.9$
- investigating a relatively disentangled language we see that extra, undisentangled message token still helps

compositional languages are easier to learn
- re-train new receiver (big GRU, small GRU, or FF) on compositional sender 
- learning speed and generalization are highly correlated to compositionality measures

## citation

```
@inproceedings{chaabouni2020-compositionality89,
    author = {Rahma Chaabouni and Eugene Kharitonov and Diane Bouchacourt and Emmanuel Dupoux and Marco Baroni},
    title = {Compositionality and Generalization In Emergent Languages},
    year = {2020},
    booktitle = {ACL 2020: 58th annual meeting of the Association for Computational Linguistics},
    pages = {4427-4442},
    publisher = {Association for Computational Linguistics}
}
```
