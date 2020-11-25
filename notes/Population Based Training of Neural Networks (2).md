---
attachments: [Clipboard_2020-11-22-20-17-43.png]
title: Population Based Training of Neural Networks
created: '2020-11-23T00:16:25.217Z'
modified: '2020-11-23T01:22:30.325Z'
---

# Population Based Training of Neural Networks

## overview

- asynchronous hyperparameter optimisization using parallel population of learners
- during training, check performance of others, `exploit` best hyperparams, `explore` new config with perturbation
- faster, better on RL for reward, machine TL for BLEU, GANs for Inception score

![](@attachment/Clipboard_2020-11-22-20-17-43.png)

## PBT

previous hyperparam methods were 
- sequential (run experiment, figure out better params, run next)
- parallel (grid search on hyperparams)

PBT is a single training optimization for a population size $N$
- when `ready` (e.g. after $k$ epochs) start PBT
- `exploit` get best hyperparams $h',\theta'$ based on current perf 
- if your hyperparams are not best, `explore` new hyperparams by perturbing $h$, copying $\theta$
- keep track of params, hyperparams, performance at step $t$

best PBT used in experiments
- *truncation selection*: if perf in bottom 20\%, sample uniformally from 20\%
- *perturb*: randomly perturb each hyperparam by factor $\in [1.2,0.8]$

## experiments

UNREAL on Deepmind Lab 
- reward

A3C on Atari
- reward

Transformer on WMT 14 EN-DE
- BLEU score

GANs on Imagenet
- Inception Score

Analysis
- if pop < 10, variance is too high
- perf increases log with population
- truncation selection is robust
- combination of hyperparam + param optimization is good
- PBT learning schedule is important, not just final hyperparams
- PBT finds annealing schedules similar to handcrafted
- all PBT winners are descendents of single init

## citation

```
@misc{jaderberg2017population,
      title={Population Based Training of Neural Networks}, 
      author={Max Jaderberg and Valentin Dalibard and Simon Osindero and Wojciech M. Czarnecki and Jeff Donahue and Ali Razavi and Oriol Vinyals and Tim Green and Iain Dunning and Karen Simonyan and Chrisantha Fernando and Koray Kavukcuoglu},
      year={2017},
      eprint={1711.09846},
      archivePrefix={arXiv},
      primaryClass={cs.LG}
}
```
