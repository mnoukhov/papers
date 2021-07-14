---
attachments: [Clipboard_2020-12-25-22-51-48.png, Clipboard_2020-12-31-21-18-31.png]
title: Emergent Reciprocity and Team Formation from Randomized Uncertain Social Preferences
created: '2020-12-26T03:36:20.669Z'
modified: '2021-01-01T02:32:23.788Z'
---

# Emergent Reciprocity and Team Formation from Randomized Uncertain Social Preferences

## overview

- randomized uncertain social preferences (RUSP)
  - train by marginalizing over all team/reward-sharing possibilities
  - add and explicitly model uncertainty over teams
- reciprocity and team formation on [GAME]


## RUSP

![](@attachment/Clipboard_2020-12-25-22-51-48.png)

each episode
- partition agents into soft teams that share reward (1-person team possible)
- assign random uniform $[0,1]$ reward within teams $T$
- agent $k$ sees noisy $T + \mathcal{N}(0,\Sigma_k)$ and uncertainty $\Sigma_k$

at evaluation
- agents are given no teams, no uncertainty $T = I, \Sigma=0$

agents
- recurrent entity-invariant 
- PPO

## experiments

2-agent, 10-step IPD shows basic direct reciprocity
- RUSP decreases self-play defect, increases all-defector defect
- soft teams (% reward sharing) necessary for reciprocity
- asymmetric noise $\Sigma_i \not = \Sigma_j$ necessary 
- past play (10% play against previous version) necesssary for stability

3-agents take turns playing IPD shows indirect reciprocity, tracking reputation
- *hold out* has agent choose action given other agents' play, still reciprocates all-defect/all-cooperate despite not training with them
- *prior rapport* plays some against cooperate, then defect, then cooperate and shows it learns to cooperate again


![](@attachment/Clipboard_2020-12-31-21-18-31.png)


team formation in 5-player *prisoner's buddy*
- standard MARL fails to form teams  
- RUSP generally finds stable teams of 2 
- splitting into discrete teams (instead of fully random $T$) is essential to learn this 

Oasis physics-based tragedy of commons env
- selfish ends up being a free-for-all with bad results
- RUSP learns to gang up 2v1 but imperfectly
- Training explicit 2v1 teams outperform RUSP

Cleanup and Harvest from SSDs
- RUSP finds cooperative equilibria
- uncertainty is not necessary


## notes

improvements after publication
- anti-social preferences ($T_{ij} < 0$) gets reputation faster
- evaluation is best with no preferences ($T = \hat T = I$) but faking max uncertainty $\Sigma = \sigma_{max}$

agents learn to form and *strongly maintain* teams but this isn't necessarily good
- 3-agent RUSP means one will always lose :/


## citation

```
@inproceedings{baker2020-reciprocity74,
    author = {Bowen Baker},
    title = {Emergent Reciprocity and Team Formation from Randomized Uncertain Social Preferences},
    year = {2020},
    booktitle = {Advances in Neural Information Processing Systems},
    volume = {33}
}
```
