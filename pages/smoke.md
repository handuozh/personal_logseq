---
title: smoke
tags: paper,[[3d Object Detection]], [[monocular]], [[mono3D]]
---

## Keywords
:PROPERTIES:
:heading: true
:END:
### #[[3D Object Detection]] [[single-stage]] #monocular #mono3D
### 评分 [[3.5💗️]]
## SMOKE: Single-Stage Monocular 3D Object Detection via Keypoint Estimation #readdone
### Metadata

#### * Item Type: [[Article]]
#### * Authors: [[Zechen Liu]], [[Zizhang Wu]], [[Roland Tóth]]
#### * Date: [[2020-02-24]]

#### [http://arxiv.org/abs/2002.10111](http://arxiv.org/abs/2002.10111)
#### * Cite key: liuSMOKESingleStageMonocular2020
#### * Tags: #CVPR #monocular #centernet #keypoint #zotero #literature-notes #reference
#### PDF Attachments
	- [Liu et al_2020_SMOKE.pdf](zotero://open-pdf/library/items/K5KHB798)

#### [[abstract]]
##### 3D object detection系统分为两部分
###### (1) 2D region proposal 生成网络
###### (2) R-CNN 结构通过获得的regions of interest来预测3D object pose
##### 本篇文章认为2D object network是冗余的,会带来巨大的error
###### 这一点跟 [[RTM3D]]的想法类似
####### ((60193c1d-a8b3-4cc1-8ee7-80577c19e580))
##### SMOKE combine a single keypoint estimate with regressed 3D vars
###### to predict a 3D bounding box
##### Multi-step disentangling method
######
#### zotero items: [Local library](zotero://select/items/1_WVIMTSKH)

#### ## Highlights and Annotations
##### SMOKE Single-Stage Monocular 3D Object Detection via Keypoint Estimation
###### Comment 8 pages, 6 figures
## Abstract
:PROPERTIES:
:heading: true
:END:
### Traditional 3D object detection -> multi-stage network
#### depends on region-based CNN ([[R-CNN]]) or [[region proposal network]]
#### Need to attach an additional network branch to
##### learn 3D information
##### or generate a** psudo point cloud** and feed it into point-cloud-detection network
#### brings in persistent noise from 2D detection
### Propose single stage network
#### learn 3D information directly from the image plane
## Abstract
### ![image.png](/assets/pages_smoke_1610504667580_0.png){:height 308, :width 601}
## 1. Backbone
:PROPERTIES:
:heading: true
:END:
### [[DLA]]
#### aggregate information across different layers
### Replace all hierarchical aggregation connections with [[Deformable Convolution Network]]
### Output [[feature map]] downsampled 4 times
### Replace [[Batch Normalization]] with [[GroupNorm]]
#### ((60066452-7c2a-4a58-9dde-3ccca0eb6c15))
## 2. Keypoint Branch
:PROPERTIES:
:heading: true
:END:
### Similar to [[Objects as points]]
#### each object is represented by one specific point
### Identify projected 3D center of the object on image plane
:PROPERTIES:
:id: 60065273-b8d6-4dc3-9c7c-a0d07595c3c4
:END:
#### 不使用2D bounding box的中心点
#### $[x \; y \; z]^{\top}$ 3D center of object in camera frame
#### $[x_c \; y_c]^{\top}$ projection of 3D points to image plane points
#### For each ground truth keypoint, the corresponding downsampled location on the [[feature map]] is computed and distributed using
##### [[Gaussian Kernel]]
##### Standard deviation based on ground truth 3D bounding boxes projection on image plane
#### 3D box is represented by 8 2D points
##### standard deviation by the smallest 2D box with $\{x_b^{min},y_b^{min},x_b^{max},y_b^{max}\}$ to encircle 3D box
## 3. Regression Branch
:PROPERTIES:
:heading: true
:END:
### Predicts the essential variables to construct 3D bounding box for each keypoint on the [[heatmap]]
### 3D information is encoded as an 8-tuple
####
$$\tau=[\delta_z \; \delta_{x_c} \; \delta_{y_c} \; \delta_{h} \; \delta_w \; \delta_{l} \; \sin{\alpha} \; \cos{\alpha}]$$
##### $\delta_z$ denotes depth offset
##### $\delta_{x_c},\delta_{y_c}$ discretization offset due to downsampling
##### $\delta_{h},\delta_{w},\delta_{l}$ residual dimensions (尺寸)
##### $\alpha$ is observation rotational angle
###### 而不是yaw angle
###### ![image.png](/assets/pages_smoke_1610941672701_0.png){:height 269, :width 286}
#### Size of feature map for regression $S_r\in{\mathbb{R}^{\frac{H}{R}\times \frac{W}{R}\times 8}}$
### Inspired by lifting transformation in [[ROI-10D]]
#### Operation $\mathcal{F}$ to convert projected 3D points to a 3D bounding box
#####
$$B=\mathcal{F}(\tau)\in{\mathbb{R}^{3\times 8}}$$
##### Depth $z$ recovered by pre-defined scale and shift parameters
###### $z=\mu_z+\delta_z \sigma_z$
### Recover position
####
$$\begin{bmatrix} x \\ y \\ z \end{bmatrix}=K_{3\times 3}^{-1} \begin{bmatrix} z \cdot (x_c+\delta_{x_c}) \\ z \cdot (y_c+\delta_{y_c}) \\ z \end{bmatrix}$$
### Recover dimension
#### use pre-calculated category-wise average dimension $[\bar{h} \; \bar{w} \; \bar{l}]$ computed over whole dataset
#### retrieve object dimension by using residual offset
#####
$$\begin{bmatrix} h\\ w \\ l \end{bmatrix}=\begin{bmatrix} \bar{h} \cdot e^{\delta_h} \\ \bar{w} \cdot e^{\delta_w} \\ \bar{l} \cdot e^{\delta_l}  \end{bmatrix}$$
## 4. Loss Function
:PROPERTIES:
:heading: true
:END:
### 4.1 Keypoint classification loss
:PROPERTIES:
:heading: true
:END:
#### penalty-reduced [[focal loss]]
##### in a point-wise manner
##### on the downsampled heatmap
#### Let $s_{i,j}$ be predicted score at [[heatmap]] locatoin $(i,j)$
#####
$$\breve{s}_{i,j}=\left\{ \begin{aligned} s_{i,j} &  & \text{if} \; y_{i,j}=1 \\ 1-s_{i,j} &  & \text{otherwise} \end{aligned} \right.$$
#### Let $y_{i,j}$ be the ground-truth value of each point assigned by [[Gaussian Kernel]]
#####
$$\breve{y}_{i,j}=\left\{ \begin{aligned} 0 &  & \text{if} \; y_{i,j}=1 \\ y_{i,j} &  & \text{otherwise} \end{aligned} \right.$$
#### loss for a single object class
#####
$$L_{\mathrm{cls}}=-\frac{1}{N} \sum_{i, j=1}^{h, w}\left(1-\breve{y}_{i, j}\right)^{\beta}\left(1-\breve{s}_{i, j}\right)^{\alpha} \log \left(\breve{s}_{i, j}\right)$$
###### where $\alpha$ and $beta$ are tunable hyper-parameters
###### $N$ is the number of keypoints per image.
###### $(1-y_{i,j})$ penalty reduction for points around the ground truth location.
### 4.2 Regression Loss
:PROPERTIES:
:heading: true
:END:
#### 8D tuple $\tau$ for 3D bounding box
##### add [[channel-wise]] activation to the regressed parameters of **dimension and orientation** at each [[feature map]] location to preserve consistency.
#### Based on lifting transformation, we define [[l1 distance]]
#### between predicted transform $\hat{B}$ and ground truth $B$
#####
$$L_{reg}=\frac{\lambda}{N}||\hat{B}-B||_1$$
##### where $\lambda$ is a scaling factor (for balancing)
###### 防止classification, regression哪一方过度dominate
#### [[Disentangling transformation]] of loss
##### 一种effective dynamic method to optimize 3D regression loss function
##### -> multi-step form
##### The 8 corners representation of the 3D bounding box is isolated into 3 different groups
###### orientation, dimension, location
### Final loss
:PROPERTIES:
:heading: true
:END:
####
$$L=L_{cls}+\sum\limits_{i=1}^3 L_{reg}(\hat{B_i})$$
##### where $i$ is the number of groups in 3D regression branch
#### The multi-step [[disentangling transformation]] divides the contribution of** each parameter group** to the final loss.
##### 3 groups: orientation, dimension, location
###
