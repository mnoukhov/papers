---
attachments: [Clipboard_2020-11-26-18-32-12.png]
title: Grounding Language in Play
created: '2020-11-26T18:29:58.069Z'
modified: '2020-11-27T03:42:40.908Z'
---

# Grounding Language in Play

## overview

- LangLfP extends learning from play to natural language
  - pair robot behaviour with language after the fact
  - train policy with SSL using human language goals
- learns effective language grounding for robotic movement
- zero-shot performance on novel instructions, languages

## intro

scalably pair robot experience with relevant human language to bootstrap instruction following
- human language instructions
- high dim continuous sensory input + actuators
- complex long-horizong robot tasks

## Lang LfP

teleoperated "play" dataset
- non-expert humans play with env

hindsight instruction pairing
- sample random robot behaviour from play
- ask human to label sample frames with language instruction

multicontext imitation learning
- train single model on *play* and *lang-play* datasets
- map image goal ($g_enc: O_t \to z$) and NL goals ($s_enc: l \to z$) to same latent space
- learn a supervised policy
  - perception module $s_t = P_\theta(O_t)$ (also used in $g_enc$)
  - language module $s_enc$
  - latent motor plans control $\pi_theta(a_t|s_t,z_t)$ imitation learning
  - average over all goals and backprop

test time human following
- human inputs instruction $l$
- closed loop follow instruction until next input

![](@attachment/Clipboard_2020-11-26-18-32-12.png)


## setup

transfer learning
- assume access to pretrained LM 
- encode input $l$ with LM to embed space
- transfer learning through embed space

3D simulated playroom env
- 8-DOF robot
- egocentric RGB bideo
- continuous internal proprio
- text channel

Baselines
- LangBC: standard multitask imitation learning with expert demo (no play)
- LfP: play, no language
- LangLfP
  - full
  - restricted to have same size data as LangBC
  - transfer using pretrained LM

## experiments

long-horizon (continuous tasks)
- LangLfP matches LfP using only NL, no frame-level supervision
- even better results with pretrained LM
- play is better than conventional demos
  - LangLfP outperforms standard LangBC imitation learning
  - much smoother qualtitatively
  - play perf scales with model capacity, demo doesn't
- some amount of compositional generalization to new tasks

knowledge transfer
- pretrained LM is best, maybe because world semantics are similar
- resilient to synonyms, whereas regular policy isn't
- instructions $\to$ translate to 16 langs $\to$ embed also works!

## citation

```
@article{lynch2020language,
  title   = {Grounding Language in Play},
  author  = {Lynch, Corey and Sermanet, Pierre},
  journal = {arXiv preprint arXiv:2005.07648},
  year    = {2020},
  url     = {https://arxiv.org/abs/2005.07648},
  pdf     = {https://arxiv.org/pdf/2005.07648.pdf},
}
```
