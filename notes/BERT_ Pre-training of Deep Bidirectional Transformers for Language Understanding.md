---
attachments: [1810.04805.pdf, Clipboard_2020-10-15-18-30-04.png, Clipboard_2020-10-15-18-30-21.png]
tags: [nlp, self-supervised]
title: BERT_ Pre-training of Deep Bidirectional Transformers for Language Understanding
created: '2020-01-15T14:44:55.571Z'
modified: '2020-10-16T00:31:12.313Z'
---

# BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding

## overview
- bidirectional transformer-encoder pretraining
- finetuning by just adding layer on top achieves many SOTA on NLP


![](@attachment/Clipboard_2020-10-15-18-30-21.png)

## model

multilayer bidirectional transformer encoder 
- base 12L, 768H, A12, 110M (same size as GPT)
- large 24L, 1024H, A16, 340M
- (layers L, hidden size H, self-attention heads A, params)

input
- WordPiece
- `[CLS]` first token, use its final hidden state
- `[SEP]` between sentences
- learned sentence embedding
- positional embedding

![](@attachment/Clipboard_2020-10-15-18-30-04.png)


## pretraining
bidirectional, not directional!

masked LM (aka cloze): mask some tokens in the sequence and predict them
1. choose 15% of the token to mask
  to account for train/test distribution difference, mask tokens 
  i. with `MASK` 80% of the time
  ii. with a random token 10% 
  iii. leave unchanged 10%
2. softmax on top of hidden state of that position predicts token


next sentence prediction
- predict whether sentence B follows sentence A
- 50% of the time replace B with a random sentence
- [CLS] hidden state to predict

data of long contiguous sequences
- BooksCorpus (800M words)
- EN Wikipedia (2500M words)

## finetuning

encode two-sentence problems as `[CLS] A [SEP] B`
- paraphrasing
- entailment
- question-passage in QA

output
- token representation for token-level task
- `[CLS]` representation for classification

add single linear layer for finetune

## experiments

7% SOTA on GLUE
- random restarts to account for instability on small dataset

1.5F1 SOTA on SQuADv1.1
- start vector $S$, end vector $E$
- dot-product between $S$ and word rep $T_i$ to get probability of start word (same for end word)
- choose highest probability start and end, giving answer span

5.1F1 SOTA on SQuADv2
- treat start,end at `[CLS]` as no answer
- predict no answer when $p$(answer) < $p$(no answer) $+ \tau$
- optimize $\tau$ on dev set

8% SOTA on SWAG
- construct four input sequences of sentence/continuation
- `[CLS]` representation * vector to get a score for each, apply softmax

## ablations
NSP and bidirectionality are both essential
- no NSP loses accuracy on NLI tasks
- left-to-right does far worse on SQUAD
- adding BiLSTM to left-to-right improves but still worse

bigger models do better
- first to show big model improvements on small-scale tasks
- key is fine-tuning and only adding few additional parameters

BERT features for CoNLL NER
- compare finetune with frozen feature + 2layer BiLSTM
- (concat of last 4 hidden)-features are competitive with SOTA

## discussion

what does BERT attend to?
- `[SEP]` gets a lot of attention as a no-op

RoBERTa
- increase sequence length
- remove NSP
- bigger batches, more data

DistilBERT
- 

### citation
```
@inproceedings{devlin2019-bert,
  title={BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding},
  author={Devlin, Jacob and Chang, Ming-Wei and Lee, Kenton and Toutanova, Kristina},
  booktitle = "Proceedings of the 2019 Conference of the North {A}merican Chapter of the Association for Computational Linguistics: Human Language Technologies, Volume 1 (Long and Short Papers)",  
  publisher = "Association for Computational Linguistics",
  year={2019}
}
```
