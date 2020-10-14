---
attachments: [Clipboard_2020-10-05-16-46-47.png, Clipboard_2020-10-05-17-39-00.png, Clipboard_2020-10-05-17-40-49.png, Clipboard_2020-10-05-17-48-45.png]
tags: [self-supervised]
title: Representation Learning with Video Deep InfoMax
created: '2020-10-05T20:37:19.800Z'
modified: '2020-10-06T02:59:30.653Z'
---

# Representation Learning with Video Deep InfoMax

## overview

- extend DIM,AMDIM to video domain
- outperform SOTA by large margin on UCF-101
- VDIM is faster, easier than previous SOTA transformers

## VDIM

![](@attachment/Clipboard_2020-10-05-16-46-47.png)

preprocessing to generate two views $G_{1,2}$ from video frames 
- get clips of same number of frames (duplicate last frame if necesary)
- split into two maybe-overlapping subsequences of 32 frames
- downsample each subsequence independently (make sure final is 32 frames)
- data augment each subsquence independently (crop, color jitter, grey, rotation)

![](@attachment/Clipboard_2020-10-05-17-40-49.png)

constrastive architecture
- 2D Conv in spatial + 1D conv in temporal model
- features $f_i$ are taken from each layer
- for each layer, apply $\phi_i$ MLP with residual to project features
- get similarity score $s_{i,i',j,j'}$ as dot product between projections 
- apply InfoNCE as lower bound to MI and optimize

![](@attachment/Clipboard_2020-10-05-17-39-00.png)

negative samples are other samples from the batch

use features from layers 5,6,9

## experiments
pretraining (left) and finetuning (right) on UCF-101, HMBD-51, Kinetics 600

![](@attachment/Clipboard_2020-10-05-17-48-45.png)

baseline hyperparams
- more views is better
- Lab dropout better than color jitter + random grey
- less downsampling does better
- found optimal learning rate and decay

VDIM hyperparameters
- you need at least layer 5 or 6 as well as 9
- color jitter + random grey 
- no rotation 
- no offset for clips 
- pretraining with temporal difference 
- downsampling of 2 helps when combined with other data augmentation

Pretraining on UCF, SOTA on UCF-101 by $2-7\%$ 

Pretraining on Kinetics, SOTA on UCF,HMDB
- beats previous results with fewer params, less compute


## citation 

```
@article{hjelm2020-representation60,
    author = {R Devon Hjelm and Philip Bachman},
    title = {Representation Learning with Video Deep InfoMax},
    year = {2020},
    booktitle = {arXiv preprint arXiv:2007.13278}
}
```

AMDIM
```
@inproceedings{bachman2019-representations57,
    author = {Philip Bachman, R Devon Hjelm and William Buchwalter},
    title = {Learning Representations by Maximizing Mutual Information Across Views},
    year = {2019},
    booktitle = {NeurIPS 2019 : Thirty-third Conference on Neural Information Processing Systems},
    pages = {15535-15545}
}
```


DIM
```
@inproceedings{hjelm2019-representations91,
    author = {R Devon Hjelm, Alex Fedorov, Samuel Lavoie-Marchildon, Karan Grewal, Philip Bachman, Adam Trischler and Yoshua Bengio},
    title = {Learning deep representations by mutual information estimation and maximization},
    year = {2019},
    booktitle = {ICLR 2019 : 7th International Conference on Learning Representations}
}
```
