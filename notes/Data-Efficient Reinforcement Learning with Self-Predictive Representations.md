---
title: Data-Efficient Reinforcement Learning with Self-Predictive Representations
created: '2020-11-05T23:17:14.031Z'
modified: '2020-11-06T00:50:44.611Z'
---

# Data-Efficient Reinforcement Learning with Self-Predictive Representations

## overview

- self-supervision for data-efficient RL
- SPR agent predict it's own latent space $n$ steps in future
- data augmentation for future prediction also shows improvement
- DER+SPR+augment new SOTA on 100k step Atari, beating 7/26 human scores

## SPR

models
- online encoder of states $z_t = f_o(s_t)$, target encoder is exponential moving average $\theta_m \gets \tau \theta_m + (1-\tau)\theta_o$
- learned transition model for online $\hat z_{t+1} = h(z_t, a_t)$ and true state for target $\tilde z_{t+k} = f_m(s_{t+k})$
- projection heads for online $\hat y_{t+k} = q(g_o(\hat z_{t+k}))$ and target $\tilde y_{t+k} = g_m(\tilde z_{t+k})$

learning algorithm
- sample $K+1$ states, actions from replay buffer 
- SPR loss as cosine sim between online and target representation $L(s_{t:t+k}) = -\sum_k \cos(\tilde y_{t+k}, \hat y_{t+k})$

data augmentation
- image augmentations of random shifts + color jitter
- 


data-efficient rainbow
- more gradient updates
- earlier replay sampling
- 20-step returns in TD-loss


important points
- without augmentation, you need dropout


## citation

```
```
