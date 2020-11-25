---
attachments: [Clipboard_2020-11-12-01-23-55.png]
title: 'Self-supervised Learning with Geometric Constraints in Monocular Video Connecting Flow, Depth, and Camera'
created: '2020-11-12T05:30:07.276Z'
modified: '2020-11-12T06:38:05.111Z'
---

# Self-supervised Learning with Geometric Constraints in Monocular Video Connecting Flow, Depth, and Camera

## overview

- GLNet: SSL for depth, optical flow, camera pose, from monocular video
- learn DepthNet, CameraNet, FlowNet and use explicit photometric, geometrics constraints between them
- SOTA on KITTI and Cityscapes, transfer to Youtube

## GLNet

learning 3D geometry
- early handcrafted work
- supervised learning using explicit data
- SSL on photometric consistency for paired images

**G**eometric **L**earning Net
- predict depth $D$, camera pose $C = (R,t)$ (+ instrinsic $K$), flow $F$
- DepthNet $\theta$, CameraNet $\alpha$, FlowNet $\delta$
- calculate next coordinates/image given current using parameters

SSL Objective
- adaptive photometric loss
  - difference between next image and prev image $\to$ 3D projection $\to$ motion predict $\to$ 2D projection
  - pixel loss minimum between optical flow prediction and rigid motion prediction 
  - pixel loss is weighted structural similarity (SSIM) and L1
- multi-view 3D structure consistency
  - penalize changes in depth prediction not consistent with transformations
  - updates between DepthNet and CameraNet
- epipolar constraint loss
  - ensure consistency between camera displacement and optical flow
  - geometric awareness for optical flow
- regularization 
  - depth smoothness
  - optical flow smoothness
  - forward-backward flow consistency

![](@attachment/Clipboard_2020-11-12-01-23-55.png)

## model

online optimization: update SSL objective on test set regularized to be close to initial pred
- parameter finetuning: update the parameters
- output finetuning: update the outputs as free variable with priors of intial pred

models
- DepthNet from single frame: ResNet encoder, DispNet deconv decoder
- CameraNet from two frames: regress 6DOF using small convnet
- FlowNet from two frames: encoder-decoder with ResNet

## experiments

training / test
- KITTI, Cityscapes
- initialize with ImageNet pretrained
- online refinement for 50 iterations

SOTA depth estimation on KITTI
- big gains when using ground-truth camera intrinsics
- inferring intrinsics works just as well (same camera used for train/test)
- GLNet w/o refinement beats others w/o it
- large SOTA when learning on Cityscape, testing on KITTI

qualitative depth estimation on Youtube videos seems good

depth estimation ablations
- each loss + refinement is necessary for big improvements (except epipolar)
- PFT is slightly better than OFT but slower, PFT + OFT is best
- online refinement really useful for transfer learning 

SOTA optical flow on KITTI
- apc and multi-view losses don't improve much over baseline, epipolar does
- no-ground-truth intrinsics does just as well
- geometric constraints sharpen rigid flow predictions

matches SOTA on pose estimation on KITTI

## citation

```
@inproceedings{chen2019-constraints62,
    author = {Yuhua Chen and Cordelia Schmid and Cristian Sminchisescu},
    title = {Self-Supervised Learning With Geometric Constraints in Monocular Video: Connecting Flow, Depth, and Camera},
    year = {2019},
    booktitle = {2019 IEEE/CVF International Conference on Computer Vision (ICCV)},
    pages = {7063-7072},
    publisher = {IEEE}
}
```
