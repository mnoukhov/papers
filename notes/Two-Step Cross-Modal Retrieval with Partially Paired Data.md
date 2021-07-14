---
attachments: [Clipboard_2021-07-12-15-32-16.png]
title: Two-Step Cross-Modal Retrieval with Partially Paired Data
created: '2021-07-12T18:54:05.313Z'
modified: '2021-07-14T18:25:16.668Z'
---

# Two-Step Cross-Modal Retrieval with Partially Paired Data

## overview 

- semi-supervised common-latent cross-modal retrieval using cycle consistency
  - avoid non-differentiable with two-step retrieval (TSR)
  - step 1 encode text, images into separate latents
  - step 2 encode latents into common latent
  - gan-like discriminator losses for "regularization"
  - supervised and unsupervised triplet loss for retrieval
- ablation study
- benefits from unpaired data from other sources


## background

cross-modal retrieval
- find semantically close text given image (image captioning)
- find semantically close image given text (image generation/selection)

approached
- CCA (old doesn't scale)
- pairwise (e.g image + text -> OSCAR/BERT...)
  - best accuracy
  - slow to search because it's a scoring function, need to compare all pairs
- hash codes
  - very fast
  - low recall because hashes are a bad latent
- common latent space
  - this paper
  - fast approximate neighbour search can mean fast lookups

## Two-Step Retrieval

![](@attachment/Clipboard_2021-07-12-15-32-16.png)

two-step autoencoding
1. cycle autoencode text,image into text,image latent
  reconstruction loss: L2 for image, teacher-forcing for text
  $L_{{AE}_v} = E_x[||x - G_v(E_v(x))||^2]$

2. cycle autoencode text,image latent into shared latent
  reconstruction loss: L2 for image,text embeddings
  $p = E_v(x)$
  $L_{{EAE}_v} = E_x[||p - G_v'(E_v'(p))||^2]$


cycle reconstruction
- encode-decode unpaired data into opposite embedding space
  image embed -> latent -> text embed -> latent -> image embed
  $L_{{CC}_v} = E_x[||p - G_v'(E_t'(G_t'(E_v'(p))))||^2]$


paired data embedding loss
- if paired data, then loss between image->text embed and paired text->text embed
  $L_{{PR}_v} = E_x[||p - G_v'(E_t'(w))||^2]$

domain adversarial
- learn discriminator on image/text embed 
  distinguish real text-> text embeds and image embed -> latent -> text embed
  $L_{{GAN}_v} = E_x[D_v'(p)] - E_y[D_v'(G_v'(E_t'(w)))]$

latent space "regularization"
1. distinguish real joint embeds from normal dist
  $L_{{GAN}_{dist}} = \frac{1}{2} E_x[D_{dist}(E_v'(p))] + \frac{1}{2} E_y[D_{dist}(E_t'(w))] - E_{z \sim \mathcal{N}(0,I\sigma)}[D_{dist}(z)]$

2. distinguish image embed -> laten from text embed -> latent 
  $L_{{GAN}_{latent}} = E_x[D_{dist}(E_v'(p))] - \frac{1}{2} E_y[D_{dist}(E_t'(w))]$

paired retrieval
1. triplet loss for paired objects
  $v = E_v'(E_v(x))$ and $t = E_t'(E_t(y))$ are latents
  distance between 2nd embedding of pair $(v,t)$ is margin $m$ or less than $(v,t')$ or $(v',t)$
2. hard negative triplet
  triplet loss using only closest pair $(v,t')$ and $(v',t)$

unpaired retrieval
1. triplet loss 
  create a pseudo-$t$-pair for $v$ with $G_t'(E_t'(v))$ and do triplet loss
2. hard negative triplet


## task

recall@K metric
- is paired object in the top K based on model's score

paired data
- Flickr30k
- MSCoco + flow split?

unpaired data
1. images + captions from dataset but unpaired
2. images from MSCoco + captions from Flickr30k
3. GRU LM trained on captions, take highest likelihood wikipedia text as captions

note that all paired data is also used unpaired

## results

partially-labelled data
- comparative results on 100% to previous common latent approaches
- better on 50%,25%,10% than VSE++ (but it doesn't use unsupervised data)
- also better than jWAE with caveat that it's not their main model (SCAN -> pairwise is and it outperforms)

substituting data
- performance scales with caption quality (best is coco then other data then wiki)
- novel unpaired data (wiki or flickr) improves performance at 100% paired coco

adding unpaired data
- performance increases with quantity of unpaired data
- diminishing returns after 75%

loss ablations
- each loss adds slight improvements 

generalization from one dataset to other
- outperforms CCA, E.Net, and jWAE (w/o SCAN)


## citation

```
```



