---
attachments: [Clipboard_2020-10-15-00-28-02.png]
title: Character-level Convolutional Networks for Text Classification
created: '2020-10-15T04:03:30.643Z'
modified: '2020-10-15T04:53:07.564Z'
---

# Character-level Convolutional Networks for Text Classification 

## overview

- character-level 1D convolutions for text classification
- on large scale data, SOTA or comparable against traditional methods, other ConvNets

![](@attachment/Clipboard_2020-10-15-00-28-02.png)

## Character-level CNN

1D convolution over characters
- max pooling
- ReLU activation
- SGD with momentum
- Gaussian weight initialization

character quantization
- alphabet size $70$ english letters, digits, punctuation, newline, space (other characters encoded as zero-vector)
- max sequence length $1014$ (extra characters discarded)

models
- 6 conv layers, 3 fc layers

data augmentation with thesaurus
- replace $r$ words with their synonym probability $p^r$
- choose synonym of semantic distance $s$ with probability $q^s$

input data is lowercased
- distinguishing between upper and lower didn't help

## experiments

### baselines

Traditional: multinomial logistic regression + 
- bag of words, TFIDF
- bag of ngrams, TFIDF
- bag of means on word2vec (5000 k-means from word2vec)

CNN
- with word2vec
- with lookup tables (end-to-end learned)

vanilla LSTM
- word2vec embeddings
- extract mean of hidden states  
- multinomial logistic regression on top

### large scale datasets

AG news corpus
Sogou news corpus (Chinese Pinyin)
DBPedia ontology
Yelp Reviews (Polarity / Full)
Yahoo Answers dataset
Amazon reviews (Polarity / Full)

### results

SOTA on larger datasets (Yahoo, Amazon, 1m+)
- convnets scale better than traditional methods
- language can be thought of as a signal
- better on user-generated data?

Struggles on smaller dataset
- AG, DBPedia, Sogou, Yelp Polarity


bag of means makes no sense

best model likely does best on sentiment analysis and on topic classification

## citation

```
@inproceedings{zhang2015character,
	title="Character-level convolutional networks for text classification",
	author="Xiang {Zhang} and Junbo {Zhao} and Yann {LeCun}",
	booktitle="NIPS'15 Proceedings of the 28th International Conference on Neural Information Processing Systems - Volume 1",
	pages="649--657",
	year="2015"
}
```
