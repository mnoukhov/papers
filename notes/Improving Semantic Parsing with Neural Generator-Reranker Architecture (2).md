---
attachments: [improving_semantic_parsing_with_neural_generator_reranker_architecture (2).pdf]
tags: [nlp, semantic]
title: Improving Semantic Parsing with Neural Generator-Reranker Architecture
created: '2020-02-16T18:01:25.118Z'
modified: '2020-02-16T21:06:58.872Z'
---

# Improving Semantic Parsing with Neural Generator-Reranker Architecture

## summary
- semantic parsing by using existing [Shaw et al (2019)](./Generating Logical Forms from Graph Representations of Text and Entities.md) model as a generator and learning to rerank final beams
- beams may be preprocessed to naturalish language and reranker is BERT possibly pretrained on Quora paraphrasing
- improves on [Overnight](./Building A Semantic Parser Overnight.md), `Geo`, and `ATIS` datasets by ~%1 over existing baseline

## model
- generator
  - train to create logical forms Shaw et al (2019) semantic parser (graph + attention)
- preprocess logical forms in one of three ways
  - no preprocessing
  - replace entity names with NL e.g `_arrival_time` -> arrival time
  - NL template for logical form e.g. `arg max(type.player, numRebounds)` -> player with largest number of rebounds
- reranker
  - BERT (not pre-trained?) critic that predicts whether each logical form is paraphrase of answer
  - rerank logical forms by how likely each is a paraphrase
  - (optional) don't rerank if logical forms score $< 0.5$
  - (optional) don't rerank if two different high-scoring logical forms

## experiments
`overnight`
- baseline Shaw et al 82.1% 
- improved to 83.7%
  - reranking with beam size 25
  - pretraining on quora paraphrase
  - don't rerank when all scores $<0.5$

`Geo`, `ATIS`
- lots of tweaking to get $<1\%$ improvement
- not really datasets that we care about any more

error analysis isn't clear, possibly cherry-picked?


## issues
- datasets are either small and out of date (`Geo`, `ATIS`) or not really meant to be a benchmark (`overnight`)
- `overnight` accuracy is execution accuracy
  - could be getting right answer with wrong method especially for such simple datasets
- only %1 improvement on the baseline on a really easy dataset
  - not really getting close to the top-10 accuracy
  - baseline isn't even meant for this dataset so its probably not SOTA if you really try
- architecture could be better
  - not training the generator and critic together
  - maybe not using pre-trained BERT
- reranking isn't novel
  - See [Yin and Neubig (2019)](./Reranking for Neural Semantic Parsing.md)

## citation
[openreview](https://openreview.net/forum?id=rkgc06VtwH)

```
@misc{
  inan2020improving,
  title={Improving Semantic Parsing with Neural Generator-Reranker Architecture},
  author={Huseyin A. Inan and Gaurav Singh Tomar and Huapu Pan},
  year={2020},
  url={https://openreview.net/forum?id=rkgc06VtwH}
}
```
