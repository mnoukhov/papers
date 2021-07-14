---
title: 'Multi-Agent Language Learning: Symbolic Mapping'
created: '2021-06-20T00:45:35.090Z'
modified: '2021-07-12T18:52:14.205Z'
---

# Multi-Agent Language Learning: Symbolic Mapping

## overview

## setup

### game

community of homogenous speak/listen agents

object datasets $O_{n,m}$
- $n$ attributes
- $m^a$ possible values for attribute $a$

pair dataset $P_{n,m}$
- pair $o_i,o_j$ are same or differ at one attribute
- label $a$ for objects differing at attribute $a$
- label $0$ if objects are identical 

discrimination game
- agents $i$ and $j$ respective of object pair $o_i,o_j$ 
- at each dialog round $t$
  - can choose to terminate dialog and guess
  - sends single symbol from vocab
- reward $1$ if agent guesses correctly, $0$ otherwise
- reward $0$ if passes round $T_max$

description game (reconstruction)
- object $o_i$
- speaker sends sequence of symbols from vocab 
- listener predicts object values $o_i$

### agents

map object to vocab
- represent object $o_i$ as concat of one-hot attributes
- FC + sigmoid to get logprobs vector of dim $|V|$
- sample several(?) symbols according to prob for each $v \in V$

input to action/message
- embed sampled symbol $s$ + concat speaker flag ($0$ self, $1$ other)
- feed into LSTM
- score each possible $w_i$ in word bank
  - embed $w_i$
  - concat with hidden $h_t^i$
  - "speaking net" MLP + sigmoid outputs score
- softmax over scores and sample message

next agent 
- feed in message
- concat hidden state and input object to decide on action 
  - if continue then talk
  - else answer get reward

description game
- no decision network???
- fixed message length $n$..$=2$

### metrics

referential disentanglement (refdis)
- similar to chaabouni 
- entropy  

referential divergence (refdiv)
- symmetry of learned communication
- JSD between agent $i$ and $j$ for the value dit of attribute $a$ given symbol $s$ input

## experiments

### LSTM vs SM+LSTM

success rate is perfect in description, terrible in discrim
- LSTM w/o SM does better than SM on discrim!

refdis
- descript: "low" in both but slightly better with SM
- discrim: very low and not really better with SM  

refdiv (only discrim)
- bad on LSTM, better on SM + LSTM
- but results on SM + LSTM are barely above random so compositionality isn't important!

### shared listener

using a shared listener increases symmetry

### vocab expansion

can agents make use of learned vocabulary for discrimination game

new attribute style with 2 values (solid, dotted)
- finetune from trained 2-attribute agents
- zero padding on object and symbol embeddings?
- reinitialize SM to dim=8 
  - set weights to 0 ??
  - load previously learned weights??

agents achieve 100% accuracy 
- as previously
- "good" compositionality
- agents tend to use new vocab items for new attributes?

## citation
```
```


## review

### intro

"symbolic mapping" is a term already used to refer to the grounding of symbols

TODO your interpretation of Tomasello, 2010 implies he suggests a curriculum of language learning 

language is a general capacity??

intuition is questionable: learning words and concepts only works if the same words and concepts are reused in other tasks

define "symmetric communication protocols" - currently you define non-symmetric and it is not clear what you mean (speakers have different mapping? )

### related work

cite lewis for referential game (signalling game)

your task formulation is not novel and is covered in many works
- early termination is seen in Cao et al 
- training both speaker and listener with RL: see EGG library
- "no game in these studies is similar to ours" how?

?? you compare to Chaabouni et al 2020 but you do not reconstruct your input

? "we turn our attention to the way agents produce symbols when communicating"

### experimental framework

?? why are there $m^a$ possible values for attribute $a$

?? why not simplify $\prod_{a=1}^n m^a = m^{\frac{n (n+1)}{2}}$

??? why are there T rounds if the game is feasible in 1

??? "may not be clear what the symbols from the partner are referring to, unless the two objects are totally the same" in a standard referential game it is completely clear that each message specifies how to differentiate one referent from other 

### symbolic mapping

??? how many words are sampled for the word bank
  - input objects (all of them?) are mapped to vocab + SIGMOID???
  - "several" are sampled  

? very strange non-standard speaking network 
  - why do you have to score each word in the vocabulary 
  - standard would be to directly learn prob distribution over vocab

??? sigmoid + softmax to get prob distribution over vocab?
  - each symbol is learned by MLP + sigmoid but then a softmax is applied over all symbols
  - applying a sigmoid is unnecessary and just saturates your gradient

? why are RNN protocols difficult to generalize

??? symbolic mapping seems no different from having a regular vocabulary
- functionally seems similar
- vocabulary expansion, compositionality, transfer -> all possible with regular setup
- "symbols should not be influenced by the dialog process" ? isn't symbolic grounding the whole point of dialog (i.e. Wittgenstein language games)
- 

### metrics

?? ignoring posdis because "meaning depending on the position" makes "language hard to transfer to dialogyue"

??? your metrics are not put into context, it is not clear what "good" score should be and whether the increase in your scores is statistically significant




### experiments 

#### 4.1

??? a dataset of 2 attributes with 3 values each is far too toyish for your setting and I have no confidence that any of your results will generalize to even a slightly larger setting

??? you differentiate your setting as multi-turn with agents ability to end early but given that there are only 2 rounds of dialogue, this is not impressive. Agents have no information 

??? why is vocab size = 6

??? slightly better refdis, not clear at all whether it is useful

???? horrible results on discrimination game are signs that your architecture and setup have real issues. the game is not that difficult so have essentially random performance is troubling

??? better refdiv with SM is not impressive if your success rate is random

#### 4.2

? bad explanation of cultural spread of language / convention

?? conventions *can* spread without a shared listener!

#### 4.3

????? "reinitialize the agents’ symbolic mapping as a linear layer with output dimension dim = 8 and set the weights to be zero"
  - SETTING WEIGHTS TO 0
  - loading new weights

? would prefer mathematical or at least visual representation of vocabulary expansion, it is not clear how this is one

??? Table 6 results are not convincing
  - 100% success rate is not surprising given previous discrim results are 100%
  - Your experiment is strongly insufficient to substantiate your point
    - language is evolved from simple to complex interactions
  - what is "good" compositionality if you don't have a baseline


??? Figure 4 is not clear and badly labelled
  - unclear where the data is coming from 
    - it is said this is from "one experiment"
    - do you mean one run of the experiment? 
  - scale of the graphs is not explained
    - y-axis says "counts" but they are frequencies
    - values are clearly normalized but not explained how


# review 

As far as I can tell, the research question this paper looks at is how to learn a compositional representation of an input space to do better generalization. This paper does not reasonably answer this question, the architecture is not as novel as it is made out to be, and the experiments strongly lack common, simple baselines to substantiate their claims.

The experimental setup is both incredibly toyish, does not use strong baselines, and I disagree with the choice of metrics. 
- Though the setup is possible for a total number of objects $\prod_{a} m^a$, the actual number of symbolic inputs needed to be represented is 9 (2 attributes with 3 values each). This is just too simple of a task as evidenced by the 100% accuracy and is not interesting for experiments using neural networks for representation. The authors should really consider using a much more complex symbolic setup like SCAN (or NACS etc..) or the simple visual environment ShapeWorld. 
- The communication setup is far too complex for how simple the game is. The whole dataset can be described in four bits but the authors create a two round speaker-listener game with an extra action to terminate speech for what amounts to an autoencoding task (or discrimination which is just xor). Why is this necessary? 
- The aim of "compositionality" without considering generalization doesn't make much sense (see Kharitonov et al.) and the metrics for the task, refdis and refdiv are not a good measure of generalization. It would have been much better to use a standard metric like TRE. The authors should justify their setup and metrics much stronger.

The architecture does not seem very novel (contrary to the paper's assertion) but the implementation is very non-standard. There are no comparisons to standard architectures and no ablations to convince me that it is necessary and that a much simpler architecture (2 agents, standard emergent communication setup) could not do much better. The architecture is also overkill for the incredibly simple task it is meant to solve.
- There is only a single baseline in the paper (LSTM) and it arguably outperforms the more complex proposed method (higher success rate on one task). 
- The compositionality metrics used are not put into context and values are shown without qualifying whether the difference is significant, especially given how small the dataset is. 
- Given that the whole dataset can be described with 4 bits, I believe an MLP would be even more effective than an LSTM.

Even if the setup was reasonable, the results presented are not strong and it is not clear how significant they are. Improvement on metrics are mild at best which is even more troubling given how toyish the task is.

Finally, the paper is very hard to understand. Methods, results, and major architectural details, e.g. word bank, are not clearly explained or illustrated. Deisgn choices are not justified for why they might help with compositionality in a concrete way. The research question is interesting but the authors would need a much more overhauled approach to demonstrate that their contributions make sense. I suggest the following major changes
- a more standard, more complex dataset (Shapeworld at minimum)
- a generalization task where compositionality would help, and a description of that specific compositionality (e.g. shape/object separation). this can be used to create a held-out test set to quantitatively measure compositionality/generalization
- strong baselines (e.g. compositional RNN work like that on SCAN) or at the very least applying your method to different architectures to show it is generally useful (e.g. LSTM + SM, Transformer + SM, etc...)
- an analysis of compositionality with TRE using the described compositionality operators

