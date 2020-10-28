---
attachments: [Clipboard_2020-10-27-10-31-50.png, Clipboard_2020-10-27-11-47-52.png, Clipboard_2020-10-27-11-48-15.png]
title: An Empirical Model of Large-Batch Training
created: '2020-10-27T14:28:31.939Z'
modified: '2020-10-27T15:48:15.402Z'
---

# An Empirical Model of Large-Batch Training

## overview

- gradient noise scale predicts largest effective batch size
- noise scale increases over training and with model size
- argues for adaptive batch-size training

![](@attachment/Clipboard_2020-10-27-10-31-50.png)

## theory

tradeoff between speed and efficiency is controlled by batch size $B_{crit} = \frac{E_{min}}{S_{min}}$
- $E_min$ is minimum examples to reach level of perf
- $S_min$ is minimum steps to reach level of perf

simplified noise scale $B_{simple} = \frac{tr(\Sigma)}{|G|^2}$ predict order of magnitude for $B_{crit}$
- trace of gradient covariance / global norm of gradient
- batch size << $B_{simple}$ is 

noise scale varies over training 
- adaptive batch size
- well-tuned temperature (e.g. Adam) is alright to keep fixed

## experiments

![](@attachment/Clipboard_2020-10-27-11-47-52.png)

![](@attachment/Clipboard_2020-10-27-11-48-15.png)

## citation

```
@article{mccandlish2018-empirical90,
    author = {Sam McCandlish and Jared Kaplan and Dario Amodei and OpenAI Dota Team},
    title = {An Empirical Model of Large-Batch Training.},
    year = {2018},
    booktitle = {arXiv preprint arXiv:1812.06162}
}
```
