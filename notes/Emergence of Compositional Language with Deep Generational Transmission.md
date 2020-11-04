---
attachments: [Clipboard_2020-11-02-15-54-37.png]
title: Emergence of Compositional Language with Deep Generational Transmission
created: '2020-11-02T20:35:58.721Z'
modified: '2020-11-03T19:07:49.057Z'
---

# Emergence of Compositional Language with Deep Generational Transmission

## overview

- symbolic 2-agent sender-receiver game in population
- periodically replace agents to encourage generational transmission
- better compositional generalization 

## setup

task and talk 
- 3 attribute types (colour, shape, style), 4 values each
- A-bot agent sees attribute values, Q-bot knows which types are asked for
- 2 rounds of *single* token communication from each agent
- then A-bot predicts attribute values

evaluation
- 4-way cross-validation(?), 4 random seeds each
- original Kottur et al, random held-out test set (e.g. green, dotted,triangle)
- new random pair held-out test set (e.g. green, *, triangle)

![](@attachment/Clipboard_2020-11-02-15-54-37.png)

population training
- population of Qs, As
- at each epoch sample 1 Q, 1 A, play a batch, update

replace generation every $E$ epochs, choose 1 Q, 1A to replace
- uniform sampling 
- lowest validation accuracy with $p = 0.8$, uniform otherwise
- oldest 

model settings
- minimal vocab ($V_Q = 3, V_A = 4$)
- memoryless + minimal vocab ($h^t_A = 0$)
- overcomplete vocab ($V_Q = V_A = 64$)
- memoryless + overcomplete

training without early stopping 
- 8 generations (199000 epochs)

## experiments

baselines
- single 2-agent scenario
- no replacement

better (but bad) results on harder held-out pairs dataset
- population-based beats 2-agent
  - argue that it is compositional because of better generalization
- replacement strategies beat no-replacement (in population) 
  - arguement for cultural transmission
  - difference among replacement strategies not huge
- memoryless + small vocab is the best
  - removing memory isn't huge

population makes agents languages similar (no shit)
- measure difference in A language over population
- KL between A_i, A_j, averaged over all i,j

visualizing grammar shows some level of disentanglement

## citation

```
@article{cogswell2019-compositional25,
    author = {Michael Cogswell and Jiasen Lu and Stefan Lee and Devi Parikh and Dhruv Batra},
    title = {Emergence of Compositional Language with Deep Generational Transmission},
    year = {2019},
    booktitle = {arXiv preprint arXiv:1904.09067}
}
```
