---
attachments: [2003.12694.pdf]
title: Countering Language Drift with Seeded Iterated Learning
created: '2020-06-11T16:58:48.048Z'
modified: '2020-06-17T20:36:54.953Z'
---

# Countering Language Drift with Seeded Iterated Learning

## overview

- solve language drift by using seeded iterated learning
- on signalling game, show it allows for better generalization without language drift
- on translation game, show it outperforms grounding through images, but does have some semantic drift

## seeded iterated learning

1. start with pretrained sender and receiver
2. create `student` and `teacher` copies of each
3. train `teacher`s together for the task with RL (interactive)
4. train `student` with supervised learning on samples from `teacher` (imitation)
5. make this `student` the newest model and start again from 2

hyperparameters
- $k_1$ number of steps of interactive
- $k_2$, $k_2'$ number of steps of imitation for sender and receiver

## signalling game

### setup
sender must communicate object to receiver 
objects are 5 properties with 5 possible values each
messages are length 5 with vocab size 25
fully connected agents, softmax output, cross entropy loss
gumbel-softmax to train through discrete 

### results
pretrain sender and receiver with cross-entropy on pretrain object and messages 

baselines
- `emergent` train from scratch on interactive data
- `gumbel` finetune on interactive data from pretrain
- `s2p` train with supervised + pretrain loss

SIL outperforms all baselines while mostly maintaining the initial pretrain language
You need to hyperparameter search for $k_1, k_2$ but there is an ok range of values
Supervised learning on teacher samples doesn't lead to overfitting, but "explicitly copying the teacher distribution"(?) does

### notes

strange default language 
- default language is $[o_1, t + o_2, ...,(p − 1)t + o_p]$
- experiment with more standard language $[o_1, o_2, ..., o_p]$

crazy systematic generalization 
- pretraining only has 10 / 3125 objects
- interactive has 30 / 3125 objects
- held-out set is 2995 / 3125 objects
- supposedly because larger pretraining has the sender and receiver fully
solve the game by using the target language

split is different
- interactive CONTAINS pretraining
- so all 10 examples in pretraining are also in interactive
- why does `emergent` do worse than `gumbel` then ?

imitation learning can't overfit?
- training on samples doesn't allow model to overfit
- likely a property of the small data 

GS $\tau$ hyperparam is not annealed over training
- could be a real issue
 
## translation game

translation game of [(Lee et al)](@note/Countering Language Drift via Visual Grounding) from FR -> EN -> DE grounded in Multi30k images

measurements
- `task score` BLEU(FR) measures success of translation pipeline
- `language score` BLEU(EN) measure language drift
- `structural drift` NLL of EN phrases under pretrained model measures syntactic drift
- `semantic drift` R1 retrieval accuracy of grounded image given 19 distractors


### results
SIL optimizes task score better than other methods while not drifting as much
- language score as good as fixed sender, syntatic drift
- R1 as good as initial model, semantic drift

S2P does nearly as well but has the tradeoff of task score vs language score

NLL of trained student (offspring) is usually lower than teacher, but graph is not super convincing

SIL is necessary throughout training for strong effects

### notes

S2P with $\alpha = 1$ is still improving at end of training
- could possibly do even better if training time is lengthened

SIL likely also has a tradeoff of task vs language
- $k_1$ vs $k_2$

Semantic drift likely still happening
- but maybe not very large?

## citation 

```
@misc{lu2020countering,
    title={Countering Language Drift with Seeded Iterated Learning},
    author={Yuchen Lu and Soumye Singhal and Florian Strub and Olivier Pietquin and Aaron Courville},
    year={2020},
    archivePrefix={arXiv}
}
```
