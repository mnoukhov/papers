---
attachments: [Clipboard_2020-10-29-14-56-37.png, Clipboard_2020-10-29-15-01-21.png, Clipboard_2020-10-29-15-18-58.png]
title: 'ViLBERT: Pretraining Task-Agnostic Visiolinguistic Representations for Vision-and-Language Tasks'
created: '2020-10-29T17:51:53.296Z'
modified: '2020-11-01T22:57:41.176Z'
---

# ViLBERT: Pretraining Task-Agnostic Visiolinguistic Representations for Vision-and-Language Tasks

## overview
- joint MLM vision + language pretraining using images + alt text
- single model finetuned beats SOTA on VQA, VCS, Caption Image Retrieval

# model

![](@attachment/Clipboard_2020-10-29-14-56-37.png)

parallel BERT models over vision + text with co-attention interaction
- transformer block on single modality
- co-attention transformer on both modalities
- text input $w_o \ldots w_T$ as usual
- image input $v_1 \ldots v_T$ from pretrained object recognition model

![](@attachment/Clipboard_2020-10-29-15-01-21.png)

co-attention
- vision rep $H_v$, word rep $H_w$
- swap keys, values so attention over $Q_v, K_w, V_w$ and $Q_w, K_v, V_v$

![](@attachment/Clipboard_2020-10-29-15-18-58.png)

masked mulit-modal modelling
- mask 15% of image, text
- masked images are zeroed 90%, unchanged 10%
- masked text is BERT (`<MASK>` 80%, random 10%, unchanged 10%)
- train on image semantic class
  - predict image semantic class distribution
  - KL between pretrained semantic class distribution

multi-modal alignment
- whether image $I$ described by text $W$
- predict = $\sigma(w * h_{IMG} \cdot h_{CLS})$
- negative sample images, captions (from batch?) 

## training details

image representation
- extract bounding boxes + features using Faster-RCNN
  - 10-36 bounding boxes with detection probability > threshold
  - features are mean-pooled conv from bounding box
- extract 5D spatial location as normalized (top-left $x,y$, bottom-right $x,y$, % of image covered)
- $v_i$ is sum of visual feats + projected spatial location

## experiment setup

VQA 2.0
- 1.1M CoCo images with 10 human answers for each
- 2 layer MLP on top of $h_{IMG} \cdot h_{CLS}$ to get distribution over 3129 possible answers
- 10 human answers per question, cross-entropy on soft target per answer
- softmax over answers at test time

Visual Commonsense Reasoning (VCR)
- 290k multiple choice questions from 110k movie scenese
- one input per choice, concat question + choice, get $h_{IMG} \cdot h_{CLS}$, linear layer to get score
- softmax over all scores, cross-entropy on the correct answer

Grounding Referring Expressions
- RefCOCO+ get bounding box given natural language 
- use Mask R-CNN proposal boxes 
- predict matching score for each box using final hidden $h_{v_i}$
- bounding box IoU > 0.5 is marked as correct
- choose highest scoring bounding box at test time

Caption-based Image Retrieval (CIR)
- Flickr 30k images, 5 captions each
- 4-way multiple choice training with 3 distractors: random caption, random image, hard negative image
- Softmax over alignment scores for each choice
- Score each caption-image pair at test time and sort

Zero-shot CIR
- no finetuning on the dataset, just score and choose max

baselines
- single stream of BERT architecture, initialized with BERT base
- ViLBERT initialized with BERT but not pretrained

## results

SOTA on all tasks!
- whereas single stream and no-pretrain are both worse
- pretraining gets you 2-10%
- more pretraining is better

Arch allows transferring from different depths
- VCR and RefCoCo want shallower ??

Zero-shot performance is decent
- shows that ViLBERT learns something!

## citation

```
@inproceedings{lu2019-visiolinguistic29,
    author = {Jiasen Lu and Dhruv Batra and Devi Parikh and Stefan Lee},
    title = {ViLBERT: Pretraining Task-Agnostic Visiolinguistic Representations for Vision-and-Language Tasks},
    year = {2019},
    booktitle = {Advances in Neural Information Processing Systems},
    pages = {13-23}
}
```
