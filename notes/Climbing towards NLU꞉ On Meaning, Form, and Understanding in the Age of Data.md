---
title: 'Climbing towards NLU: On Meaning, Form, and Understanding in the Age of Data'
created: '2020-12-08T17:54:59.749Z'
modified: '2020-12-09T02:49:56.228Z'
---

# Climbing towards NLU: On Meaning, Form, and Understanding in the Age of Data

## overview

- meaning is the functional use of language, while LMs can only capture form
- thought experiments demonstrate that our method won't scale to true understanding
- focus on the right questions, climb the right hills

## meaning and learnability

definitions
- *form* is the observable realization of language
- *meaning* maps form $e$ to intent $i$, $M \subseteq E \times I$
- *conventional* meaning is constant across contexts

LMs only see form, not intent
- therefore can't learn meaning
- their understanding of meaning is like Searle's Chinese Room

Octopus test
- A and B communicate with eavesdropping Octopus
- A talks about new catapult, O's response is generic but A attributes meaning to it
- A asks for help attacked by bear, O's response is *likely* phrase but not *useful* 
- without ability to actually see intent, O is not learning meaning

Program execution by LM
- learn to execute Java code using only source, not input or output
- learn to VQA given only unconnected text and images
- only meaning-understanding system can execute code, answer questions

Children must have *joint-attention* and *interaction* to learn a language

Distributional semantics loses in favour grounding, "meaning is use"

## the proposal

Learn and *test* on meaning
- make sure that you're not subverting the test with hidden patterns

Climb the right hill
1. ask top-down questions about direction
2. be aware of task (dataset) limitations
3. carefully create new, difficult tasks
4. evaluate meaning across tasks
5. carefully analyse errors and successes

## counterarguments

meaning is hard to define
- this definition is useful and syntax/semantic dependency is insufficient

meaning can be learned from grounding
- yes, but it must be explicitly coded along with form

enough data will scale
- perfectly likely responses are still not perfectly useful
- memorizing all combinations is not NLU

neural representations *do* capture meaning
- they capture *aspects* of meaning, but not sufficient for all of it
- unparallel NMT may invalidate hypothesis (*or not!* if you assume both languages have similar distributions)

BERT improves performance on meaning-related tasks
- it clearly learns *something* about meaning but we don't know how much

## citation

```
@inproceedings{bender2020-understanding75,
    author = {Emily M. Bender and Alexander Koller},
    title = {Climbing towards NLU: On Meaning, Form, and Understanding in the Age of Data.},
    year = {2020},
    booktitle = {Proceedings of the 58th Annual Meeting of the Association for Computational Linguistics},
    pages = {5185-5198},
    publisher = {Association for Computational Linguistics}
}
```


