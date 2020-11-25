---
attachments: [Clipboard_2020-11-10-11-21-10.png, Clipboard_2020-11-10-17-22-43.png]
title: Planning to Explore via Self-Supervised World Models
created: '2020-11-10T05:09:51.644Z'
modified: '2020-11-10T22:32:00.486Z'
---

# Planning to Explore via Self-Supervised World Models

## overview

- Plan2Explore SSL RL world model exploration policy from imagined trajectories
- SOTA on zero-shot, adaption on 20 control tasks
- ablations on few-shot learning, generalization, future vs retrospective

## background

intuition
- curiosity trains on what you've already seen, not what you have to explore
- instead learn a world model to plan ahead and intrinsically be curious
- seek inputs that you learn the most from, be robust to what you can't learn

PlaNet
- image encoder $h_t = e_\theta(o_t)$
- posterior dynamics $q(s_t | s_{t-1}, a_t, h_t)$
- prior dynamics $p(s_t | s_{t-1}, a_t)$
- reward predictor $p(r_t | s_t)$
- image decoder $p(o_t | s_t)$

Dreamer on latent states to learn action $\pi(a_t | s_t)$, value $V(s_t)$ networks

## Plan2Explore
![](@attachment/Clipboard_2020-11-10-11-21-10.png)

![](@attachment/Clipboard_2020-11-10-17-22-43.png)

latent disagreement
- approximate information gain using ensemble of one-step predictors $q(h_{t+1} | w_k, s_t, a_t)$
- each part of ensemble is separately init and sees data in different order, trained with MSE
- curiosity is disagreement, variance of means $Var(\mu(w_k, s_t, a_t)| k \in [1:K])$

## experiments

DM control suite

baselines
- Dreamer (world model with rewards)
- Curiosity (unsupervised)
- model-based active exploration (unsupervised)

Plan2Explore is unsupervised SOTA
- zero-shot beats curiosity and MAX
- can sometimes match Dreamer, which has oracle rewards

competitive with Dreamer using 100-150 few-shot
- P2X unsupervised 1k episodes + few shot
- beats it in hopper, not as good in cartpole

generalizes better to novel tasks
- trained on cheetah run, test on run backwards, flip
- P2X gets better during training, beats Dreamer, random explore

expected disagreement (world model) better than retrospective (curiosity)
- P2X beats one-step agents

## citation

```
@article{sekar2020-supervised58,
    author = {Ramanan Sekar and Oleh Rybkin and Kostas Daniilidis and Pieter Abbeel and Danijar Hafner and Deepak Pathak},
    title = {Planning to Explore via Self-Supervised World Models},
    year = {2020},
    booktitle = {arXiv: Learning}
}
```
