---
title: 'On the Dangers of Stochastic Parrots: Can Language Models Be Too Big?'
created: '2021-04-08T15:36:00.456Z'
modified: '2021-04-08T18:05:03.900Z'
---

# On the Dangers of Stochastic Parrots: Can Language Models Be Too Big?

## overview
- larger and larger LMs are getting SOTA on benchmarks but there are drawbacks
  - environmental impact of training
  - using large, uncurated internet text may impose biases
  - not actually moving towards NLU, just form-level
- recommendations

## environmental 
energy cost of training model
- is >> regular human energy cost
- not using green energy???

climate change is bad
- and authors blame LMs!

## large uncurated data

large internet data overrepresents certain viewpoints
- more white supermacists/sexists online than in person?
- younger and from developed countries
- male (and worse, from Reddit)

training data probably doesn't include 
- people who are harassed more ?
- niche blogs + websites 
- people using "bad" words in good ways

existing data biases towards existing viewpoints
- retraining on new data is too difficult??

biases from society are encoded into learned models
- toxicity is difficult to prevent explicitly
- model auditing requires knowing bias axes ahead of time and it is still difficult to find all correlations
- all biasing choices are political??

solution is manual documentation
- use similar resources to archival data collection???

## LMs aren't NLU

opportunity cost of not working with carefully curated data?

bigger models are "tricking" us into thinking they understand meaning

## risks

- amplify biases from input
- be mean to people online
- make fake news / conspiracies seem bigger
- trusting NMT systems that can make errors

## notes

### environment 

a lot of dumb straw man arguments
- comparing carbon emissions of a human and training a research model, why not compare to other research? physics simulation maybe?
- 0.1 BLEU increase = $150k...so I guess it isn't economically worth it do!
- Maldives underwater???? Is this a research paper or a soapbox? 

free market counterargument
- as long as training neural models is economically worth it, they will be trained
- if you don't like the environmental impact, focus on making the cost of energy include that impact 
- orgs will naturally choose the best size: environmental argument against LMs specifically makes no sense

arguing against climate change while pointing the blame at LMs makes no sense
- if you want to point the blame, then make an argument for their share of environmental impact

### large uncurated data

questionable arguments about data representation
- are there more white supremacists online than in person?
- reddit in 2016 isn't reddit in 2018
- who is using Twitter data and how does an anecdote mean anything?
- everyone is harassed on twitter!
- "using lots of data biases towards popular viewpoints"
 
how do you choose which "marginalized voices" must be heard
- filtering out nazi communities and filtering in BLM communities requires an explicit value judgement
- requiring better representation you require an arbiter of "truth/correctness"
- you want your LM to represent your own viewpoint but what will actually happen is it will represent the dominant viewpoint of whatever org creates it

documentation/data collection is not feasible! 
- most academia does not have the budget to data collect as recommended
- no economic incentive to do so for companies, way to identify issues for users
- impossible to do large scale data collection like this

no concrete solutions proposed
- other than extensive manual curating
- even this type of curation is not immune to the issues mentioned

### not NLU

just because it's not NLU doesn't mean it doesn't work
- CNNs are not image understanding but they can contribute to image understanding

philosophical question that isn't really pertinent to an engineering approach
- unless you're arguing for causality / ToD-BERT training

### risks

issues of LMs are just issues of "neural networks" and correlation-based ML

## citation

```
@inproceedings{bender2021-stochastic33,
    author = {Emily M. Bender and Timnit Gebru and Angelina McMillan-Major and Shmargaret Shmitchell},
    title = {On the Dangers of Stochastic Parrots: Can Language Models Be Too Big?},
    year = {2021},
    booktitle = {Proceedings of the 2021 ACM Conference on Fairness, Accountability, and Transparency},
    publisher = {ACM}
}
```
