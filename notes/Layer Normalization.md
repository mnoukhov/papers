---
title: Layer Normalization
created: '2020-02-07T19:09:56.533Z'
modified: '2020-02-10T17:57:15.524Z'
---

# Layer Normalization

## summary

- batch norm but computed across hidden units instead of batch
- no need to keep track of running mean,var for inference time

## citation

```
@misc{ba2016layer,
    title={Layer Normalization},
    author={Jimmy Lei Ba and Jamie Ryan Kiros and Geoffrey E. Hinton},
    year={2016},
    eprint={1607.06450},
    archivePrefix={arXiv},
    primaryClass={stat.ML}
}
```
