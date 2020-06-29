---
title: Reasoning about Pragmatics with Neural Listeners and Speakers
created: '2020-06-25T18:50:10.852Z'
modified: '2020-06-26T16:08:15.809Z'
---

# Reasoning about Pragmatics with Neural Listeners and Speakers

## overview

- image captioning that explicitly reasons about listeners (pragmatics)
- learning pragmatics through interaction w/o pragmatic data
- 

## models

literal listener
- embed both images $e_1,e_2$ and the description $e_d$
- pass through linear and get softmax
- pretrained on true image, distractor image, true description

literal speaker 
- basic neural captioning based on true image embedding $e_i$
- does not condition on distractor
- pretrained on image, true description

reasoning speaker
- draw samples from literal speaker
- score samples with literal listener $p_k = p_{L0}(i|d_k, r1, r2)$
- choose sample that leads to highest probability of success ($\argmax p_k$)

NL reasoning speaker
- keep language close to human using tradeoff $\lambda$ between listener and speaker
- $pk = p_{S0}(d_k|r_i)^\lambda \cdot p_{L0}(i|d_k, r_1, r_2)^{1−\lambda}$

compiled speaker
- embed and concat true and distractor image $[e_i, e_{-i}]$
- train on true image, distractor image, reasoning speaker output
- essentially use reasoning to bootstrap this model

## citation

```
@inproceedings{andreas2016reasoning,
    title = "Reasoning about Pragmatics with Neural Listeners and Speakers",
    author = "Andreas, Jacob  and
      Klein, Dan",
    booktitle = "Conference on Empirical Methods in Natural Language Processing",
    year = "2016",
    publisher = "Association for Computational Linguistics",
}
```
