---
title: ASLFeat
published: true
permalink: aslfeat
public: true
---

## Meta Data
:PROPERTIES:
:heading: true
:END:
### #title ASLFeat: Learning Local Features of Accurate Shape and Localization, 2020
### #topic  #Keypoint #[[Descriptor Matching]] [[D2-Net]] [[R2D2]]

## Limitations of local feature detection and descriptions
### Awareness of estimating local shape (scale, orientation, etc)

### Localization accuracy

## Literature Review:
:PROPERTIES:
:heading: true
:END:
### [[Lf-net]] and [[D2-Net]] empirically yield low precision in two-view matching or introduce large reprojection error in SfM tasks. As the detections are from low-resolution feature maps ($$ 1/4 $$ times original size).

### [[Superpoint]] learns to upsample the feature maps with pixel-wise supervision from artificial points.

### [[R2D2]] employs [[dilated conv]] to maintain the spatial resolution but trades off excessive GPU and memory usage.
#### It handcrafted a selection rule to derive keypoints from the same feature maps that are used for extracting feature descriptors, which ^^couples^^ the capability of detectors and descriptor. ^^Good^^  

### Detections from ^^deepest^^ layer might not be able to identify low-level structures (corners, edges, etc) where keypoints are often located.

## Contributions:
:PROPERTIES:
:heading: true
:END:
### [[Deformable Convolution Network(DCN)]] in dense prediction framework. 

### Allow pixel-wise estimation of lcoal transformation as well as progressive shape modelling by stacking multiple DCNs.
#### Leverage inherent feature hierarchy. Restore both spatial resolution and low-level details for accurate keypoint localization.

### Peakiness measurement for more selective keypoint detection.

## Network Architecture
:PROPERTIES:
:heading: true
:END:
### ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FSLAM%2FaeNGx2iSyq.png?alt=media&token=5b050b88-2111-4dc9-81c0-d892ca64f1f8)

## 1. Prerequisites
:PROPERTIES:
:heading: true
:END:
### Output features $\mathbf{y}$ of a standard convolution for each spatial position $\mathbf{p}$ is:
#### ^^(1)^^                 $$ \mathbf{y(p)}=\sum\limits_{\mathbf{p}_n\in\mathcal{R}} w(\mathbf{p}_n)\cdot\mathbf{x(p+p_n)} $$, 
given regular grid $\mathcal{R}$ sampling over input feature map $\mathbf{x}.$
### DCN augments the regular convolution by additionally learning both sampling offsets $\{\Delta\mathbf{p}_n|n=1,\cdots,N\}$ and feature amplitudes $\{\Delta\mathbf{m}_n|n=1,\cdots,N\}$, where $ N=|\mathcal{R}|$, and rewrite Eq. (1) as:
#### ^^(2)^^                   $$ \mathbf{y(p)}=\sum\limits_{\mathbf{p}_n\in{\mathcal{R}}}w(\mathbf{p}_n)\cdot \mathbf{x(p+p_n}+\Delta\mathbf{p_n})\cdot\Delta\mathbf{m}_n $$  
with feature amplitude $$ \Delta\mathbf{m}_n $$ limited to $$ (0,1) $$.

## 2. DCN with Geometric Constraints
:PROPERTIES:
:heading: true
:END:
### Original free-form DCN predicts local transformation of DOF like $9\times 2$ offsets for a $3\times 3$ kernel. It can model complex deformation but takes a risk of over-parameterizing local shape.
### Affine-constrained DCN. Decompsed as
#### ^^(3) ^^             $$ \mathbf{S}=\lambda R(\theta)=\lambda\left( \begin{array}{cc}\cos(\theta) & \sin(\theta) \\ -\sin(\theta) & \cos(\theta) \end{array}\right) $$  

### WIth additional estimate of shearing, as a learnable problem by [AffNet](AffNet.md):
#### ^^(4)^^              $$  \begin{aligned} \mathbf{A} = & \mathbf{S}A^{\prime}=\lambda R(\theta)A^{\prime} \\= & \lambda \left( \begin{array}{cc}\cos(\theta) & \sin(\theta) \\ -\sin(\theta) & \cos(\theta) \end{array} \right) \left( \begin{array}{cc}a_{11}^{\prime} & 0 \\ a_{21}^{\prime} & a_{22}^\prime   \end{array} \right)\end{aligned} $$
where $det(A^{\prime})=1$. The network predict one scalar for scaling, two for rotation and other three for shearing ($A^{\prime}$).
### Homography-constrained DCN
#### TODO To read this part  

## 3. Selective and Accurate Keypoint Detection
:PROPERTIES:
:heading: true
:END:
### Refer to original [[D2-Net]]:
#### Section
##### Amenable for back propagation.

##### Define a soft local-max score
###### (4)            $$ \alpha_{ij}^k=\exp(D_{ij}^k)/\sum_{(i^{\prime},j^{\prime})\in\mathcal{N}(i,j)} \exp(D_{i'j'}^k) $$
where $\mathcal{N}(i,j)$ is the set of 9 neighbours of pixel $(i,j)$.
##### Define a soft channel selection to compute a [Ratio-to-max](Ratio-to-max.md) per descriptor that emulates channel-wise non-maximum suppression:
###### (5)            $$ \beta_{ij}^k = D_{ij}^k/\max\limits_t D_{ij}^t $$  

##### Together we maximize the product of both scores across all feature maps $k$ to obtain single score map:
###### (6)            $$ \gamma_{ij}=\max\limits_k (\alpha_{ij}^k \beta_{ij}^k $$)

##### Image-level normalization:
###### (7)             $$ s_{ij}=\gamma_{ij} / \sum\limits_{(i',j')} \gamma_{i'j'} $$

## Keypoint peakiness measurement
### The [[Ratio-to-max]] (Eq. (5)) has the limitation that it only weakly relates to the actual distribution of all responses along the channel.

### We modify it with ^^Peakiness^^ as keypoint measurement:
#### ^^(5)^^            $$ \beta_{ij}^c=\text{softplus}(\mathbf{y}_{ij}^c - \frac{1}{C}\sum\limits_t \mathbf{y}_{ij}^t ) $$  
where softplus activates the peakiness to a positive value.

#### Similarly, for Eq. (4) we rewrite:

#### ^^(6)^^             $$ \alpha_{ij}^c=\text{softplus}\left(\mathbf{y}_{ij}^c-\frac{1}{|\mathcal{N}(i,j)|}\sum\limits_{(i',j')\in\mathcal{N}(i,j)}\mathbf{y}_{i'j'}^c \right) $$