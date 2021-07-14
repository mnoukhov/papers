---
title: '"This item is a glaxefw, and this is a glaxuzb": Compositionality Through Language Transmission, using Artificial Neural Networks'
created: '2021-02-07T02:11:08.216Z'
modified: '2021-02-08T03:40:29.543Z'
---

# "This item is a glaxefw, and this is a glaxuzb": Compositionality Through Language Transmission, using Artificial Neural Networks

## overview

- propose iterated learning for RNNs using sender-receiver extension
- 


## Iterated Learning with NN

Original ILM (Kirby et al, 2003)
1. DCG teacher language $G$ maps meaning $M$ to utterance $U$
2. sample meanings $M_{train}$ and use teacher to get $U_{train}$
3. train student on $G_{train}$ and generalize to $G_{test}$
4. student becomes teacher and repeat

Replacing DCG with RNN leads to degeneration
- all meanings are eventually mapped to one utterance

## Neural ILM 
sender-receiver RNN architecture ensures meaning -> utterance -> meaning 
1. sample meanings $M_{train}$ and generate utterances with sender teacher $U_{train} = f_{T,send}(M_{train})$
2. train student sender on $M \to U$ 
3. train student receiver on $U \to M$
4. train student sender-receiver on $M \to \cdot \to M$ autoencoding
5. student becomes teacher and repeat

## experiments

### training setup

- all training is done for $n$ epochs or until accuracy $acc$ is reached 
- utterances can be probability dist (`SOFTMAX`) or sampling + REINFORCE (`RL`)
- compositionality evaluation using top sim $\rho$ and holdout accuracy 

### symbolic input

setup
- 10 values x 5 symbols or 33 values x 2 symbols 
- 128 holdout, no 3+ similar attributes in training set??
- GRU with utterance length 6, vocab size 4??

results are not compositional either way
- slightly better results with ILM but always bad either way
- $10^5$ results are worse with ILM in terms of acc???

### image input

setup
- openGL images with 3D objects
- sender and receiver images have the same object-colours
- 4 distractor images with different object-colours
- 4096 training images, 512 holdout (unseen combinations)

results
- better with ILM but again very bad
  - probably just not learning well enough in the first place
- only results with 1 object are at all decent 
- worse compositionality as space gets bigger?
- variance across runs is very high
- $\rho$ is not a good measure


## notes

TRE should have been used as the metric 
- the arguments about compositionality are not strong otherwise
- this could be trivial compositionality (Korbak et al, 2020)

does not cite Lu et al (2019) or even reference it
- SIL should definitely be a baseline or at least tried

"uniqueness" measure of 3.1 is not explained
- entropy of the distribution would make more sense 

how is `SOFTMAX` evaluation done?
- is it still softmax at eval time because then it isn't a discrete protocol

$acc_h$ does *not* necessarily depend on compositionality (see Kharitonov et al (2020))

test set requirements are weird
- only 128? no 3+ similar attributes? 
- just make it systematic or not, this is a weird halfway point

