---
attachments: [1905.08205.pdf]
favorited: true
tags: [nlp, semantic]
title: IRNet
created: '2020-01-15T14:46:27.131Z'
modified: '2020-01-20T23:43:23.556Z'
---

# IRNet 

## overview

tackling text-to-SQL on the Spider dataset in three steps
- schema linking to figure out column/table names
- grammar-based neural model to create IR *SemQL* query
- deterministically infer SQL from SemQL

SOTA on Spider, even better using BERT instead of GLoVe

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

### Schema encoder
encode db schema of (column $c$, type $\phi$), tables $t$ into columns rep $E_c$, tables rep $E_t$
- base column embedding $\hat e^i_c$ is average of word/span embeddings in column name
- column type embedding $\varphi$
- context embedding $c^i_c$ is attention over span embeddings

## citation 

Towards Complex Text-to-SQL in Cross-Domain Database with Intermediate Representation
Guo et al
