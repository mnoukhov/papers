---
attachments: [Clipboard_2020-10-21-22-13-14.png]
tags: [emergent]
title: On (Emergent) Systematic Generalisation and Compositionality in Visual Referential Games with Straight-Through Gumbel-Softmax Estimator
created: '2020-10-22T01:28:36.760Z'
modified: '2020-10-22T04:20:36.830Z'
---

# On (Emergent) Systematic Generalisation and Compositionality in Visual Referential Games with Straight-Through Gumbel-Softmax Estimator

## overview

- testing compositionality on sender-receiver visual referential game with distractors
- showing that results differ for straight-through GS with visual input as opposed to regular REINFORCE + symbols
- extrapolation is harder than interpolation for visual but not symbolic

## setup

visual distractor sender-receiver game
- K = 47 distractors train, 63 test

dSprites dataset: generated heart sprite
- X, Y coordinate (8 values each)
- scale (6 values)
- rotation (8 values)

![](@attachment/Clipboard_2020-10-21-22-13-14.png)

train-test split
- interpolation: alternating train/test on each axis
- extrapolation: first values on each attribute axis

communication channel
- complete: vocab size 8 + EOS token, message len # attributes
- overcomplete: vocab size 100, message len 20

## experiments

interpolation task benefits from visual
- symbolic test is bad regardless of extrapolation or interpolation
- visual task is similarly bad on extrapolation but better on interpolation for some reason, not explained

smaller batch size gets better topograpic similarity ???
- "small batch sizes are a bottleneck"???
- contrary to ILM, bigger batch sizes are better???

bigger channel leads to better compositionality ???
- bigger topographic similarity for overcomplete than complete

no correlation between generalization and compositinality
- statistical tests are not significant either way???


## citation


