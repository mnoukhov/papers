---
title: Countering Language Drift with Seeded Iterated Learning
created: '2020-04-05T17:57:09.310Z'
modified: '2020-04-06T02:32:18.574Z'
---

# Countering Language Drift with Seeded Iterated Learning

## overview

- pretraining language followed by RL optimization on task causes language drifting
- Seeded Iterated Learning method (SIL) for countering language drift

## method

SIL
- initialize student agent
- teacher: student after $k_1$ steps RL training
- offspring: student after $k_2$ steps SL training on teacher
- student $\gets$ offspring and start again

## experiments

signalling game
- input object with $p$ properties each of size $t$
- sender message of length $p$ with vocab $t$
- language score measuring adherence to initial language 
- task score measuring task completion

data
- pre-train data
- training data
- held out data

### results

- pre-trained agent (SL on pretrain???) gets 65% baseline on task and language
  - why does the baseline language get 65%
  - this means that its language is already drifted!
- hyperparameter sweeped SIL beats 


## issues
general
- why do you use straight through GS when you can do expectation over regular GS
- $\tau$ should be scheduled not fixed, not the important hyperparameter
- S2P isn't the method in the ICLR 2020 paper

signalling game
- all hyperparameters fixed, especially $p = t = 5$ and $k_i$
- SEEDS???
- $k$ should depend on learning rate and batch size
- how does optimizing the task directly (emergent comm) do worse than any other method on the task score???
- "the final task score is higher than the language score" IS NOT LANGUAGE DRIFT
- learning rates are tuned for SIL
- YOUR TRAINING SPLIT IS NOT ABOUT LANGUAGE DRIFT!!!
  - you have a super small training split that is less about language and more about generalizing to unseen combinations of data

## citation

```
@misc{lu2020countering,
    title={Countering Language Drift with Seeded Iterated Learning},
    author={Yuchen Lu and Soumye Singhal and Florian Strub and Olivier Pietquin and Aaron Courville},
    year={2020},
    eprint={2003.12694},
    archivePrefix={arXiv},
    primaryClass={cs.AI}
}
```
