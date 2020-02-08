---
attachments: [1706.03762.pdf]
title: Attention is all you need
created: '2020-02-07T15:35:59.908Z'
modified: '2020-02-08T02:47:59.023Z'
---

# Attention is all you need

## summary
- introduces a sequence modelling arch based solely on attention, the `transformer`
- since it does not rely on recurrence, it is more parallelizable and efficient to train
- demonstrate new SOTA on English-German and English-French MT

## attention
- `attention` of a query $Q$ over some key-value pairs $K,V$. output is weighted sum of values, where the weighting is determined by compatibility of each key with the query
- `self-attention`: relating different positions of a single sequence in order to compute a representation of the sequence

`scaled dot-product attention`
- softmax$(\frac{QK^T}{\sqrt(d_k)})V$
- $d_k$ is dimensionality of keys, scaling allows softmax to have good gradients

`multi-head attention`
- reduce dimension of each input by $\frac{1}{h}$
- then do $h$ different attention mechanisms on same input
- concat resulting output to get back same dimensionality 

`masked multi-head self attention`
- for decoding need to prevent output information from being used
- mask out (set to $-\infty$) all self-attention connections to to the right


## position
- since model does not do recurrence, we must add position information
- add `position encodings` to input embeddings

`sine/cosine position encodings`
- position encodings with same dimensionality as input embeddings
- alternate `sin`,`cos` for dimensions of the encoding
- $PE_(pos,2i) = \sin(\frac{pos}{1000^(2i/d_{model})})$
- $PE_(pos,2i+1) = \cos(\frac{pos}{1000^(2i/d_{model})})$
- $PE_(pos+k)$ can be expressed as a linear function of $PE_(pos)$ to enable attending to relative positions

`position-wise feed-forward`
- feed forward net applied across *positions* (instead of just across batches)

## model
- encoder consisting of multi-head self-attention, feed-forward with residual connections, layer norm in between
- decoder consisting of masked multi-head self-attention, mult-head attention over the encodings, feed-forward with residual connections and layer norm in between

## experiments
- 1% SOTA on EN-DE with small model ($>10^3\times$ reduced computation)
- 2% SOTA on EN-DE, .5% on EN-FR with big single model ($>10^2\times$ reduced computation)

## citation

```
@incollection{vaswani2017attention,
  title = {Attention is All you Need},
  author = {Vaswani, Ashish and Shazeer, Noam and Parmar, Niki and Uszkoreit, Jakob and Jones, Llion and Gomez, Aidan N and Kaiser, \L ukasz and Polosukhin, Illia},
  booktitle = {Advances in Neural Information Processing Systems 30},
  editor = {I. Guyon and U. V. Luxburg and S. Bengio and H. Wallach and R. Fergus and S. Vishwanathan and R. Garnett},
  pages = {5998--6008},
  year = {2017},
  publisher = {Curran Associates, Inc.},
  url = {http://papers.nips.cc/paper/7181-attention-is-all-you-need.pdf}
}
```
