---
attachments: [1810.04805.pdf]
tags: [nlp]
title: BERT_ Pre-training of Deep Bidirectional Transformers for Language Understanding
created: '2020-01-15T14:44:55.571Z'
modified: '2020-02-10T18:12:53.436Z'
---

# BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding
## summary
- bidirection transformer-based pretraining

## pretraining
masked ML
- mask some tokens in the sequence and predict them
- choose 15% of the token to mask
- replace it with `MASK` 80% of the time
- replace with a random token 10% 
- leave unchanged 10%

next sentence prediction
- predict whether sentence B follows sentence A
- 50% of the time replace B with a random sentence

### Citation
```
@inproceedings{devlin2018bert,
  title={BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding},
  author={Devlin, Jacob and Chang, Ming-Wei and Lee, Kenton and Toutanova, Kristina},
  booktitle={NAACL},
  year={2019}
}```
