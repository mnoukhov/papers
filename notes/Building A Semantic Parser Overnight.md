---
attachments: [P15-1129.pdf]
tags: [nlp, semantic]
title: Building A Semantic Parser Overnight
created: '2020-01-14T22:59:28.444Z'
modified: '2020-09-08T19:37:46.535Z'
---

# Building A Semantic Parser Overnight

## overview
- Berant and Liang (2014) showed semantic parsing via paraphrasing
- This paper bootstraps the necessary text-to-parse dataset
  - define NL phrase for each predicate
  - generate full, natural-ish sentences using grammar rules
  - crowdsource paraphrase of natural-ish to natural
  - train natural phrase to logical form parser
- Then evaluates how well that bootstrap dataset does

## method

### assumptions
- database is set up and schema is fixed
- *canonical compositionality* all logical forms can be composed using a small grammar
- *bounded non-compositionality* NL utterances are compositional for some bounded size of fragments

### grammar
- *seed lexicon* $L$ : canonical phrase (NL) -> predicate (DCS)
- *grammar* $G$: set of rules for composing predicates and therefore canonical phrases
- $GEN(G \cup L)$ generates the set of all $(z,c)$ pairs
- use lambda DCS to query the database

### generation
- unary predicates are verbs e.g. "has a degree"
- binary predicates are
  - relationals e.g. "publication date of"
  - verbs e.g "rules" or "is president of"

- generate using grammar rules
  - AFAIK use just 2 of each type to generate all *combinations* (as opposed to all exact phrases)
  - perform type checking e.g. "article that cites ~~2004~~"
  - limit amount of recursion e.g "article that cites the person who took the ..."
  - specify context of relations e.g. "number of assists (over a season)"

- does not handle nested quantification or anaphora

### paraphrasing

- AMT workers reformulate or say it is incomprehensible

- *alternations of single phrases*
  - "author of article" -> "who wrote article"
- *sublexical compositionality* multiple rules replaced with single word aka multiple rules replaced with single word
  - "parent of alice whose gender is female" -> "mother of alice"

### semantic parsing

- string matching to find initial predicates $T(x)$
- generate candidate logical forms with beam search (max 20)
- log-linear model over features
- basic features match words and bigrams if aligned in PPDB
- lexical features use Berkeley Aligner (Liang et al, 2006) to learn paraphrases e.g. "fewest" == "least number"

## evaluation

### seven domains
10 errors from each domain
- 70% paraphrasing model mistakes e.g. "sit outside" -> "takes reservations"
- 12.5% reorder issues e.g. "venue with two articles" -> "article with two venue"
- 17.5% AMT worker error

### new domain
creating a semantic parser on Geo880 without using their training examples
- accuracy 56.4% (compared to ~90% SOTA)
- issues with max depth of compositions
- room for improvement

### testing completeness
does the grammar construction account for all possible questions?
- collect free-form queries from AMT for *Calendar*
- 52% are not possible to annotate with grammar
  - 20% relative time reference e.g. "next meeting"
  - 14% predicates not in grammar e.g. "agenda"
  - 2% unsupported calculations e.g. "free time"
  - 26% out of scope
- evaluate on remaining data
  - 46.3% accuracy
  - 84.2% *oracle accuracy*: correct answer was on final beam

## citation

```
@inproceedings{wang2015building,
    title = "Building a Semantic Parser Overnight",
    author = "Wang, Yushi  and
      Berant, Jonathan  and
      Liang, Percy",
    booktitle = "ACL",
    year = "2015"
}
```
