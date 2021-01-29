---
title: RTM3D
tags: paper, [[mono3D]], [[3D Object Detection]], [[single-shot]]
---

## Meta Data
:PROPERTIES:
:heading: true
:END:
### Title: RTM3D: Real-time Monocular 3D Detection from Object Keypoints for Autonomous Driving #toread, #精读
### Zotero Metadata

#### * Item Type: [[Article]]
#### * Authors: [[Peixuan Li]], [[Huaici Zhao]], [[Pengfei Liu]], [[Feidao Cao]]
#### * Date: [[2020-01-10]]
#### source [http://arxiv.org/abs/2001.03343](http://arxiv.org/abs/2001.03343)
#### #code https://github.com/Banconxuan/RTM3D
#### Cite key: liRTM3DRealtimeMonocular2020a
#### Tags: #3D-object-detection, #keypoint, #monocular, #single-shot #zotero #literature-notes #reference
#### PDF Attachments
	- [Li et al_2020_RTM3D.pdf](zotero://open-pdf/library/items/NCHMGMTE)

#### [[abstract]]:
##### monocular 3D detection framework in [[single-shot]]
##### 传统方法
###### **projection constraint** from the 3D bounding box to the 2D box
####### 4 edges of a 2D box -> 4 constraints
####### the performance **deteriorates dramatically** with the small error of the 2D detector. 2D的微小误差会导致3D严重恶化
##### 提出的方法
###### 预测 9 perspective keypoints of a 3D bounding box in image space
###### utilize the **geometric relationship** of 3D and 2D perspectives to recover the dimension, location, and orientation in 3D space.
##### 效果
###### the properties of the object can be predicted **stably** even when the estimation of keypoints is very noisy,
####### enables us to obtain fast detection speed with a small architecture.
###### Training uses the 3D properties of the object 不需要 external networks or supervision data.
###### [[KITTI]] benchmark 数据集
#### zotero items: [Local library](zotero://select/items/1_I3PQK79E)
## Structure
:PROPERTIES:
:heading: true
:END:
### ![Keypoint detection architecture](/assets/pages_rtm3d_1611822344140_0.png){:height 521, :width 657}
### output main center heatmap, vertexes heatmap, and vertexes coordinate
## Backbone
:PROPERTIES:
:heading: true
:END:
### [[ResNet]] and [[DLA]]
### Downsample input image with $S=4$ factor
### Upsample bottleneck thrice by 3 [[bilinear interpolation]]s and $1\times 1$ convolutional layer
### After upsampling, the channels are 256, 128, 64.
## Keypoint [[Feature Pyramid]]
:PROPERTIES:
:heading: true
:END:
### [[FPN]] is not suitable because keypoints in the image have not different in size
#### 作者认为FPN更适合不同尺度的物体检测,对于keypoint没有意义
### Propose new **KFPN**
:PROPERTIES:
:heading: true
:END:
#### ![image.png](/assets/pages_rtm3d_1611892704096_0.png){:height 347, :width 602}
#### detect scale-invariant keypoints in the **point-wise space**
#### $F$ scale feature maps, resize each scale $f$ [[?]]
##### $1 < f <F$ back to the size of maximal scale
##### yield feature map $\hat{f}_{1<f<F}$
#### 生成soft weight by a [[softmax]] 为每个scale分配重要性
#### Final scale-space score map $S_{score}$ by **linear weighting sum**
#####
$$S_{score}=\sum\limits_f \hat{f} \odot \rm{softmax}(\hat{f})$$
##### $\odot$ is element-wise product
## Detection Head
:PROPERTIES:
:heading: true
:END:
### 3 fundamental components
### 6 optional components
#### arbitrarily selected to boost accuracy with a little computational consumption
### 1. Inspired by [[centernet]], take a keypoint as the **main-center**
#### one point connects all features
#### Since the 3D projection point may exceed image boundary
##### 我们选择2D box center point
#### [[heatmap]] defined as $M\in{[0,1]^{\frac{H}{S}\times\frac{W}{S}\times C}}$
##### $C$ is the number of object categories
### 2. Another [[heatmap]] $V\in{[0,1]^{\frac{H}{S}\times\frac{W}{S}\times 9}}$
#### 9 perspective points projected by vertexes and center of 3D bounding box
### 3. For keypoints association of one object, regress an local offset $V_c\in{R^{\frac{H}{S}\times\frac{W}{S}\times 18}}$
#### from the main-center as an indication.
#### Keypoints of $V$ closest to the coordinates from $V_c$ 被认为一个object上
#### $9\times 2$ offset用来约束9个vertexes点的位置
### prior infomation可以进一步约束9个点
#### discretization error for each keypoint
##### center offset $M_{os}\in{R^{\frac{H}{S}\times\frac{W}{S}\times 2}}$
##### vertexes offset $V_{os}\in{R^{\frac{H}{S}\times\frac{W}{S}\times 2}}$
####
##
### [[Questions]]
### [[Research Ideas]]
### [[Permanent notes]]
## [[Writing skills]]
###
###