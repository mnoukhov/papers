---
attachments: [Clipboard_2020-11-05-18-16-44.png, Clipboard_2020-11-05-18-51-19.png]
title: Curiosity-driven Exploration by Self-supervised Prediction
created: '2020-11-05T21:46:09.136Z'
modified: '2020-11-06T00:05:44.327Z'
---

# Curiosity-driven Exploration by Self-supervised Prediction

## overview

- curiosity for learning in sparse-reward RL domains
- show better performance on VizDoom, Super Mario Bros
- demonstrate agents learns reasonable even without rewards
- demonstrate generalizable transfer + faster learning for other exploration

## curiosity-driven exploration

curiosity for exploration
- instrinsic reward based on difficulty of predicting consequence of actions
- only predict changes that (1) can be controlled or (2) affect the agent

Intrinsic Curiosity Module
- learned feature encoding $\phi(s_t)$
- predict action $\hat a_t = g(\phi(s_t), \phi(s_{t+1}))$
- predict future features $\hat \phi(s_{t+1}) = f(\phi(s_t),a_t)$
- intrinsic reward is future error $r^i_t = \frac{\eta}{2}||\hat \phi(s_{t+1}) - \phi(s_{t+1})||^2_2$ with scaling $\eta > 0$ 

![](@attachment/Clipboard_2020-11-05-18-16-44.png)

## experiments

environments
- VizDoom MyWayHome finding a target in 3D
- Super Mario Bros, four levels 
- pixel resized 42x42, grayscale, 3 frames concatenated

models
- A3C with 4layer Conv, followed by LSTM, 2layer FC
- A3C + ICM 
- A3C + ICM in pixel space (no inverse model)

ICM does best on sparse rewards

![](@attachment/Clipboard_2020-11-05-18-51-19.png)

ICM is most resistant to 

## follow-up ()

large scale study found good features
- compact: lower dimensional
- sufficient: have all the information
- stable: change as little as possible

compared different feature models (VAE, IDF, RF, Pixels)
- intrinsic usually helps

## citation

```
@inproceedings{pathak2017-exploration22,
    author = {Deepak Pathak and Pulkit Agrawal and Alexei A. Efros and Trevor Darrell},
    title = {Curiosity-Driven Exploration by Self-Supervised Prediction},
    year = {2017},
    booktitle = {2017 IEEE Conference on Computer Vision and Pattern Recognition Workshops (CVPRW)},
    pages = {488-489},
    publisher = {IEEE}
}
```
