---
title: GPT 1 and 2
created: '2020-10-22T18:09:33.207Z'
modified: '2020-10-26T16:53:51.257Z'
---

# GPT 1 and 2

## overview 

- decoder-only transformer stack for generative language pretraining
- GPT1: a single model can be finetuned for a variety of tasks
- GPT2: a larger model can learn decent zero-shot performance with scale 

## GPT 1

generative pretraining task 

model
- stack of transformer decoders layers 
- 100M parameters

data
- BooksCorpus for large stretches of contiguous text
- ftfy + spacy tokenizer
- `[START] text1 [DELIM] text2 [EXTRACT]` input 

linear finetuning from the same model leads to SOTA performance!

SOTA on certain language benchmarks (later beaten by BERT)

## GPT2

### changes

scaling up GPT1 for 0-shot SOTA
- bigger corpus
- bigger model
- bigger compute
- more efficient text extraction

WebText 40GB
- get all reddit links with >3 karma, extract text
- using n-gram bloom filters, find that the overlap between WebText and tested datasets to be minimal, similar to current train/test overlap

byte-level BPE
1. count ftfy+spacy word frequencies
2. break words into 256 byte vocabulary
3. merge two existing vocab tokens to create a new one 
  merge on most frequent new token
  repeat!
4. prevent merging across character categories (except spaces), prevent `[dog][!]` $\to$ `[dog!]`   

### results

SOTA Language modelling
- uses invertible detokenizer which removes artifacts
- best PPL/ACC on 7/8 datasets
- far from SOTA on 1BW likely due to sentence shuffling

SOTA on children's book test (cloze for omitted word)
- performance increases with model size

SOTA on LAMBADA predicting final word of long text

SOTA on Winograd Schema
- but it's a small dataset so not super impressive

Not good on classically supervised tasks
- Reading comprehension (CoQA) worse than supervised BERT 
- Translation about equal to word-by-word substitution on EN-FR but unsupervised SOTA on FR-EN
- Question answering improves with scale but still much worse than hybrid retrieval systems

Note that performance is similar to *zero-shot* supervised


## citation

```
```
