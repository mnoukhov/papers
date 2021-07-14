---
title: Reviews
created: '2021-04-16T00:24:19.998Z'
modified: '2021-04-19T16:46:16.300Z'
---

# Reviews

# Competition 1

## summary

- (potentially-interactive) instruction following for building 3D picture in minecraft enviornment
  - architect has design must communicate it
  - builder is given communication, must build
- novelty is in 
  - live interactivity* architect must correct the builder, builder can ask clarifying questions
  - evaluation with humans at test time  

## tasks

### architect
learn an agent that, given design, can communicate commands

baseline 

evaluation 
- automatic with BLEU score
- human evaluation (love it!)

### silent builder

builder that can not ask clarifying questions
- reward for getting closer to the answer
- neg reward to going further

### interactive builder

## protocol

## logistics

great that you can provide azure credits! and also mentorship! would like more details about this



## technical/organizational

Task 1 baseline code needs to be easier to use and up to date
- e.g. according to README, Task 1: Architect code doesn't support batching or attention, uses outdated pytorch
- ideally it is more plug-and-play so participants only need to modify a specific file (algorithm/architecture) and training/data-loading is taken care of

Task 2 baseline code makes sense, using a Gym environment + RLlib is logical

could silent builder disregard instructions and just use reward to guide learning during training?


