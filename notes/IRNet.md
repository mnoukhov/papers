---
attachments: [1905.08205.pdf]
tags: [nlp, semantic, sql]
title: IRNet
created: '2020-01-15T14:46:27.131Z'
modified: '2020-02-20T15:52:45.625Z'
---

# IRNet
Towards Complex Text-to-SQL in Cross-Domain Database with Intermediate Representation

## overview

tackling text-to-SQL on the Spider dataset in three steps
- schema linking to figure out column/table names
- grammar-based neural model to create IR *SemQL* query
- deterministically infer SQL from SemQL

SOTA on [Spider](@note/Spider.md), even better using BERT instead of GLoVe

## method

### schema linking
recognize the column and table names mentioned in a question
- assign a type: table, column, or value
- allows for better OOD performance

for NL encoder
- enumerate all 1-6 ngrams (possible *spans*)
- exact or subset match on columns, then tables
- match anything in quotes to a value

for schema encoder
- column/table types
  - `EXACT MATCH` if exact match
  - `PARTIAL MATCH` if subset
- values types
  - check for value "is a type of"/"related terms" column in ConceptNet
  - `VALUE EXACT MATCH` if exact match
  - `VALUE PARTIAL MATCH` if partial

### NL encoder
encode sentence of (span $x$, type $\tau$) into sequence of hiddens $H_x$
- e.g. ("book titles", `Column`) ...
- $e_x^i$ is average of span and type embedding
- $H_x$ is concat of BiLSTM hidden states over embeddings

### schema encoder
encode db schema of (column $c$, type $\phi$), tables $t$ into columns rep $E_c$, tables rep $E_t$
- base column embedding $\hat e^i_c$ is average of word/span embeddings in column name
- column type embedding $\varphi$
- context embedding $c^i_c$ is attention over span embeddings

### decoder
use [Yin and Neubig (2017)]() decoder to generate SemQL
- LSTM that takes sequential decisions based on grammar and tree structure

coarse-to-fine
- first generate `sketch` of SemQL up to aggregation
- then generate final SemQL from the best `sketch` beam

pointer networks for cols, tables
- memory-augmented pointer network to `SelectColumn`, if selected removed from schema and added to mem
- pointer network to `SelectTable`

### BERT
encode questions, database schemas, and schema linking results
- input sequence of spans [sep] distinct column names split by [sep]
- representation of span is average BERT hidden of words and type
- representation of column is BiLSTM over BERT hidden of of words + sum of type embedding

## experiments

### spider

new SOTA on Spider 55% on test set (v2 + BERT)
- schema linking very important
- BERT gives about 8% boost

semQL "effectiveness"
- shows that baseline approaches learn semQL easier than SQL
- **caveat:** does not actually show semQL leads to accuracy, just easier to learn

### error analysis
438 errors on development set

32.3% columns names that don't appear but based on cell values
- column name not mentioned in question, but cell value in that column is
- `db content used` probably necessary

15.2% columns names partially appear (e.g. synonym)

23.9% complicated nested queries
- "extra hard" level questions
- data augmentation might help

12.4% mistakes in the operator, require common knowledge
- "sort age from old to young" -> descending order

BERT fixes 30% of failures, mostly column name and common sense

Using a pseudo-test set, they find that there is a performance gap meaning they might be overfitting to the dev set

## citation

```
@inproceedings{GuoIRNet2019,
  author={Jiaqi Guo and Zecheng Zhan and Yan Gao and Yan Xiao and Jian-Guang Lou and Ting Liu and Dongmei Zhang},
  title={Towards Complex Text-to-SQL in Cross-Domain Database with Intermediate Representation},
  booktitle={Proceeding of the 57th Annual Meeting of the Association for Computational Linguistics (ACL)},
  year={2019},
  organization={Association for Computational Linguistics}
}
```

