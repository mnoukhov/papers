---
attachments: [Clipboard_2020-12-25-22-51-48.png]
title: Emergent Reciprocity and Team Formation from Randomized Uncertain Social Preferences
created: '2020-12-26T03:36:20.669Z'
modified: '2020-12-26T03:55:11.305Z'
---

# Emergent Reciprocity and Team Formation from Randomized Uncertain Social Preferences

## overview

- randomized uncertain social preferences (RUSP) environment augmentation
- train over distribution of possible reward-sharing etc
- reciprocity and team formation on [GAME]


## RUSP

![](@attachment/Clipboard_2020-12-25-22-51-48.png)

each episode
- partition agents into soft teams (% reward share) $T$
- assign random uniform $[0,1]$ reward within teams
- agent $k$ sees noisy $T + \mathcal{N}(0,\Sigma_k)$ and uncertainty $\Sigma_k$



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
