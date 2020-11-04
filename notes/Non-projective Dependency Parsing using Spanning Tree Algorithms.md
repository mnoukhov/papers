---
attachments: [Clipboard_2020-11-02-17-05-28.png, Clipboard_2020-11-02-17-05-58.png, Clipboard_2020-11-02-17-38-53.png, Clipboard_2020-11-02-17-51-25.png, Clipboard_2020-11-02-17-53-59.png]
title: Non-projective Dependency Parsing using Spanning Tree Algorithms
created: '2020-11-02T21:36:00.071Z'
modified: '2020-11-03T00:04:56.631Z'
---

# Non-projective Dependency Parsing using Spanning Tree Algorithms

## overview

- *non-projective* dependency tree parsing as search for MST in directed graph
- whereas McDonald et al (2005) use Eisner (1996) in $O(n^3)$, this paper extends to non-projective and uses Chu-Liu-Edmond making in $O(n^2)$ 
- better performance on languages with non-projective dependencies

## intro

dependency tree, one parent (another word, or root)

projective if there is no crossing in the tree
- word and descendents are contiguous substring
- more common for flexible word-order langs (German)

![](@attachment/Clipboard_2020-11-02-17-05-58.png)

![](@attachment/Clipboard_2020-11-02-17-05-28.png)

comparison to previous work
- focuses on non-projective as well 
  - Eisner (1996) used edge based factorization for projective trees but was in $O(n^3)$ because it is bottom-up to maintain projectivity
- searches the full space of non-projective trees
- reduces to spanning search tree and finds an $O(n^2)$ algorithm as opposed to exponential previously


## dependency parsing and spanning trees

edge-based factorization
- sentence $x_{1 \ldots n}$, dependency tree $(i,j) \in y$ if dependency from $i \to j$
- edge score $s(i,j) = w \cdot f(i,j)$ where $f$ is feature rep
- dependency tree score $s(x,y) = sum_{i,j \in y} s(i,j)$ sum of all scores

maximum spanning tree (MST)
- directed graph $G = (V,E)$ with directed edges $(i,j) \implies v_i \to v_j$
- MST $\argmax_y \sum_{i,j \in y} s(i,j)$ s.t. every vertex is in $y$

for each sentence $x, G_x = (V_x, E_x)$ is the full directed graph with all vertices (root + words) and directed edges between every two words
- MST of $G_x$ equivalent to dependency tree for $x$
- projective has restrictions, non-projective search with no restrictions

learn MST using Chu-Liu-Edmonds
- each vertex selects incoming edge with highest weight
- if not a tree, find the cycle, contract into a single vertex, call recursively
- $O(n^3)$ naive, $O(n^2)$ Tarjan (1977) for dense graphs

![](@attachment/Clipboard_2020-11-02-17-38-53.png)
![](@attachment/Clipboard_2020-11-02-17-51-25.png)

## online large margin learning

![](@attachment/Clipboard_2020-11-02-17-53-59.png)

Margin Infused Relaxed Algorithm (MIRA) to learn the $w$ for scoring
- training set $(x_t, y_t)$ where all possible trees are $dt(x_t)$
- loss of tree $L$ is number of words with incorrect parents
- keep new weight $w^{i+1}$ as close as possible to $w$ while correctly classifying example
- make scores approximate loss $L$
- auxiliary $v$ accumulates $w^i$ and final is average of $v$, essentially $w^{i}$ over training
- **issue** exponentially many possible parses $dt(x_t)$ so exponentially many constrains for line 4

Single-best MIRA
- fix exponential by using margin constraint on only best scoring tree $y$
- $y' = \argmax_{y'} s(x_t,y')$ 
- McDonald et al (2005) used $k$-best but that is apparently inefficient

Factored MIRA
- exploit structure to factor constraints
- weight of correct incoming edge to $x_j$ must be $\geq 1$ more than all other (incorrect) incoming edges
- recovers previous loss metric
- McDonald et al (2005) use it for projective but performs worse than $k$-best

## experiments

Czech Prague Dependency Treebank (PDT)
- uses reduced POS tags from Collins to simplify
- follows McDonald et al (2005) for generating features
- 23% of data have at least one non-projective dependency
- only 2% of edges are non-projective
- Czech-A is entire PDT, Czech-B only 23% includes non-projective

Results
- Czech: SOTA 
  - beats projective algorithm in McD(2005) and non-projective N&N (2005)
  - slightly better accuracy on A
  - demonstrably better accuracy on B shows non-projective gains
  - slight 1% edge in completely correct predictions A
  - huge 15% to 0% edge in completely correct B
  - factored model generally better
- English: worse on projective data
  - McD (2005) has slight accuracy edge, big complete edge
  - prior projective bias is useful for english

## questions

summary

strengths

- dependency trees as MST and non-projectivity using CLE alg
  - works on non-projective trees compared to Eisner (1996)
  - uniform and simple, lexicalized phrase structure algs require pruning
  - much faster than Nivre and Nilsson (2005) and using *true* non-projectivity instead of pseudo-projective parser
  - shows non-projective is easier since there are fewer constraints

limitations

  - not using projective constraints can find bad trees (sec 4?)


what is projectivity
- how is fig 2 non-projective
- what kind of structure leads to non-projectivity

dependency tree edges can be drawn without crossing
- word and descendents form contiguous substring
- substring 

what is edge factorization
- advantages
- disadvantages

which parts could be implemented by an NN
- costs 
- benefits



## citation

```
@inproceedings{mcdonald2005-projective27,
    author = {Ryan McDonald and Fernando Pereira and Kiril Ribarov and Jan Hajic},
    title = {Non-Projective Dependency Parsing using Spanning Tree Algorithms},
    year = {2005},
    booktitle = {Proceedings of Human Language Technology Conference and Conference on Empirical Methods in Natural Language Processing},
    pages = {523-530},
    publisher = {Association for Computational Linguistics}
}
```
