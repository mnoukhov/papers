---
attachments: [Clipboard_2020-10-29-13-50-17.png, electra_pre_training_text_encoders_as_discriminators_rather_than_generators.pdf]
tags: [nlp, self-supervised]
title: 'ELECTRA: Pretraining Text Encoders as Discriminators rather than Generators'
created: '2020-03-28T19:06:50.114Z'
modified: '2020-10-29T17:50:17.728Z'
---

# ELECTRA: Pretraining Text Encoders as Discriminators rather than Generators

## overview 
- replaced token detection pretraining task instead of MLM
  - generator G makes replacements instead of `[MASK]` 
  - discriminator D classifies whether each token is replaced or original
- more efficient than XLNet, RoBERTa and matches performance

## method

![](@attachment/Clipboard_2020-10-29-13-50-17.png)

### models and training
a random mask `m` is generated for input `x`

generator G is a transformer trained with MLM on input
- input `x` where tokens `m` are replaced with `[MASK]`
- predicts masked tokens, trained with ML on NLL loss

disciminator D is a transformer trained with replaced token detection (RTD)
- input `x` where tokens `m` are replaced with sample from G
- predicts whether each token is replaced ("fake") or original ("real")
- (if generator predicts correct token it is considered real)

trained together but *not* like GAN
- generator trained with ML
- no backprop from D to G
- after pretraining throw out generator

### extensions

weight sharing between G and D
- sharing embed weights if G is smaller
- token embeddings probably better in MLM

smaller generator
- smaller layer size, everything else constant
- too strong G may make it hard for D to learn

two-stage training *(not useful)*
- train D for `n` steps
- freeze D, load G with D weights and train for `n` steps

## experiments

ELECTRA-small performs better than other small models
- better than comparable BERT-small
- much better than larger GPT, ELMo
- quicker to get high performance using less compute

ELECTRA-large performs equally with 1/4 of the time, better with full time
- equals RoBERTa, XLNet with 1/4 of the training time on GLUE, SQUAD
- outperforms slightly with same number of steps 

Ablations show that improvements come from:
- learning over all tokens (not just `[MASK]`)
- eliminating pre-train w/ `[MASK]`, finetune w/o discrepancy

## conclusion

ELECTRA can be viewed as scaled up Word2Vec!
- discriminating task is like NCE
- generator for masked words instead of uniform negative examples
- transformer instead of bag-of-words

Takeways
- RTD is more efficient than MLM in training time
- Works well with smaller compute


## citation
```
@inproceedings{clark2020electra,
  title = {{ELECTRA}: Pre-training Text Encoders as Discriminators Rather Than Generators},
  author = {Kevin Clark and Minh-Thang Luong and Quoc V. Le and Christopher D. Manning},
  booktitle = {ICLR},
  year = {2020}
}
```
