---
title: Synergies between Functional and Structural Language Learning
created: '2020-06-17T20:37:10.134Z'
modified: '2020-06-22T18:04:03.662Z'
---

# Synergies between Functional and Structural Language Learning

## overview

- looks at taking a pretrained language model and adapting it to a task with self-play
- removes syntactic drift by reranking valid utterances for messages
- makes distinction between syntactic, semantic, and pragmatic drift (iffy)

## game

sender-receiver game to distinguish an image from a distractor
abstract scenes dataset
- 10k synthetic cartoon images
- average 6 captions each

### baselines
human baseline without distractors
- best 'structural' knowledge
- people likely choose the most specific caption

human baseline with distractors
- upper bound on gameplay

### speaker 
agent
- convert image to embeddings $u$ with pretrained resnet and trained hidden layer
- generate message with LSTM, add $u$ to context at each time step (why not attention?)
- gets reward 1 or -1 and update with `REINFORCE`

oracle
- `random` chooses a random gold caption
- `discriminative` chooses the caption with least overlap of distractor

### listener
- convert image to embeddings $u$ with pretrained resnet and trained hidden layer
- convert text to embedding $v$
- convert both images and pick that which has highest dot product with $v$

`joint` is trained with speaker
`fixed` is trained with `discriminative` speaker and fixed

## learning

functional
- emergent communication
- conditions on both true image and distractor
- update speaker with REINFORCE on listener 1,-1 rewards
- messages are tokens of vocab size 100, length 10

structural
- image captioning
- conditions on only true image
- update speaker using cross-entropy on true caption
- decoding scheme is either `greedy` or `sample` 

reward finetune
- pretrain image captioning, finetune emergent
- KL loss between pretrained model and finetune to counter language drift
- speaker *not* conditioned on distractor

multi-task
- $\lambda_f L^func + \lambda_s L^struct$
- seemingly also *not* conditioned on distractor

reranker
- use REINFORCE to learn to rerank 20 beams of fixed image captioning

PoE reranker
- multiply learned rank by normalized prob of beam sample
- 


## notes
reward finetune DOESN'T condition on distractor
- not exactly finetuning with functional?
- why not pass BOTH images through captioner and get min loss (closer caption)




## citation
```
@inproceedings{lazaridou2020multiagent,
    title={Multi-agent Communication meets Natural Language: Synergies between Functional and Structural Language Learning},
    author={Angeliki Lazaridou and Anna Potapenko and Olivier Tieleman},
    year={2020},
    booktitle={ACL},
}
```
