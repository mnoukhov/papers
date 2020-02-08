---
attachments: [1409.0473.pdf]
tags: [arch, nlp]
title: Neural Machine Translation by Jointly Learning to Align and Translate
created: '2020-02-07T15:43:39.796Z'
modified: '2020-02-08T00:48:05.725Z'
---

# Neural Machine Translation by Jointly Learning to Align and Translate

## summary
- NTM systems are restricted by having to encode a variable length sentence into a fixed length bottleneck
- instead, encode the sentence to a sequence and adaptively choose a subset while decoding aka the `attention` mechanism 
- first neural MT system to match phrase-based on English-French

## attention
- bidirectional RNN to encode source sentence to $h_1 \ldots h_T$
- decoder's RNN state $s_{i-1}$ as query vector 
- compute alignment b/w $h_j$ and $s_{i-1}$ using feedforward net $a$ 
- softmax over alignment is the attention over $h$

## experiments
- slightly better bleu than best phrase-based system w/o UNK
- attention corresponds well to MT word alignment
- attention outperforms seq2seq on long sentences

## citation

```
@inproceedings{Bahdanau2015NeuralMT,
  title={Neural Machine Translation by Jointly Learning to Align and Translate},
  author={Dzmitry Bahdanau and Kyunghyun Cho and Yoshua Bengio},
  booktitle={ICLR},
  year={2015}
}
```
