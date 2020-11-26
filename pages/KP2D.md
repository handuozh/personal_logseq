---
title: KP2D
public: true
---

## Meta Data
:PROPERTIES:
:heading: true
:END:
### #title NEURAL OUTLIER REJECTION FOR SELF-SUPERVISED KEYPOINT LEARNING, 2020, ICLR
### #[[topic]]: [[Keypoint]] [[Descriptor-Matching]] [[Keypoint-Extraction-and-Description]] [[Unsuperpoint]] [[Self-supervised]] [[Encoder-decoder]] [[Siamese Network]]

### #code https://github.com/TRI-ML/KP2D

## Abstract
:PROPERTIES:
:heading: true
:END:
### The learning framework is based on [[Unsuperpoint]] in a [[Self-supervised]] fashion by receiving source image $$ I_s $$ ($$ K(I_s)=\{p_s,f_s,s_s\} $$ ) and target image $$ I_t $$ ($$K(I_t)=\{p_t,f_t,s_t\} $$). 

### We define [[IO-Net]]::InlierOutlierNet

### KeyPointNet a keypoint-netowrk architecture that is especially amenable to robust keypoint detection and description.

## [[Self-supervised]] Keypoint Learning
:PROPERTIES:
:heading: true
:END:
### **Notation**
#### Input image $$I\in{\mathbb{R}^{3\times H \times W}}$$

#### Define $$K:I\rightarrow {\{\mathbf{p,f,s}\}}$$
##### keypoint $$\mathbf{p}=\{ [u,v]\}\in {\mathbb{R}^{2\times N}}$$

##### keypoint scores $$\mathbf{s} \in \mathbb{R}^K$$

##### descriptor $$\mathbf{F}\in \mathbb{R}^{256\times N}$$

##### $$N$$ total number of keypoints extracted (varies based on input resolution)

### Based on [[Unsuperpoint]] with source image $$I_s$$ and target image $$I_t$$ related via a known homography transformation $$\mathbf{H}$$.
#### ((5fbf7cce-8da7-48ce-880f-5565d06351ba))
##### Define $$\mathbf{p}_t^*=\{[u_i^*,v_i^*]\}=\mathbf{H}(\mathbf{p}_s)$$ with $$i\in I$$ the corresponding locations of source keypoints after **warping**.

### Inspired by [[Neural-Guided RANSAC]] method define a function $$C$$ 
#### Take input [[point-pair]]s along with associated weights according to a distance metric

#### Output ^^likelihood^^ each [[point-pair]] belongs to an inlier set of matches.

#### Mapping $$C: \{\mathbf{p_s,p_t^*},d(\mathbf{f_s,f_t^*})\}\in \mathbb{R}^{5\times N}\rightarrow \mathbb{R}^N$$ is the likelihood.

#### $$C$$ is only used in training phase to choose an optimal set of consistent inliers, and encourage the gradient flow through consistent [[point-pair]]s.

### ![](https://remnote-user-data.s3.amazonaws.com/ms3yhslJzZPWxUNrfg3XSH9RbSgXkP3y-f9GvX2aejwqjwvRmlwAt8pU1OqGBPXb2lctrVDIlva8SzmsdIFY3YBzDFtMgXllswRgfY8lzbhLHgvxq54mP1cWg7BBu_NR)  
#### Framework for [[Self-supervised]] keypoint detector and descriptor learning using [[KeyPointNet]] and [[IO-Net]].
##### [[IO-Net]] is a [[1d Convoluation]] parametrized by $$\theta_{IO}$$ with similar strucuture of [[Neural-Guided RANSAC]] with 4 default setting [[residual blocks]] and activation function for final layer removed. 
###### Produces an indirect supervisory signal to [[KeyPointNet]] targets by propagating gradients from the classification of matching input point-pairs.

#### Define the model $$K$$ parametrized by $$\theta_K$$ as encoder-decoder style network. Similar to [[Unsuperpoint]]: ((86d644eb-eefc-40c6-8dcc-8c88f818a9b5)).

## 1. [[KeyPointNet]]::
### a keypoint-network architecture that is optimized in an end-to-end differentiable manner by imposing explicit loss on each of the 3 target outputs (score, location and descriptor).

### ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FSLAM%2FTOQXLiMVDX.png?alt=media&token=738ec2fc-32a6-4d09-9758-56440c18df9b)

### **  Detector Learning**
#### Same points as [[Unsuperpoint]] ((5fbf7cce-e8c2-4f6f-b73f-4f00fa094e09))
##### Define distance $$ d_k $$ as
 (1)    $$L_{loc}= d_k=||T\hat{\mathbf{p}}_k^A - \hat{\mathbf{p}}_k^B|| =$$$$||T\hat{\mathbf{p}}_k^{A\rightarrow B} - \hat{\mathbf{p}}_k^B||$$
#### Effectively aggregate keypoints across cell boundaries, by mapping the relative cell coordinates $$[u_s^{'},v_s^{'}]$$ to input image:
##### (2)    $$[v_i,u_i]=[\text{row}_i^{center},\text{col}_i^{center}]+[v_i^{'},u_i^{'}]\frac{\sigma_1(\sigma_2 -1)}{2}$$

##### $$v_i^{'},u_i^{'}\in (-1,1)$$

##### cell size $$\sigma_2=8$$ and $$\sigma_1$$ is a ratio relative to the cell size (bigger than 1 can predict across cell borders).

#### Predict keypoint locations w.r.t. the cell center, and the predicted keypoints drift across cell boundaries.
##### ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FSLAM%2FIzwp9mn0iV.png?alt=media&token=76e44397-411f-42a0-a738-854c8c267d41)

##### Warped point (red) is associated to multiple predicted points (blue) based on distance threshold (dashed circle)
###### (a) [[Unsuperpoint]]  forces kepoint in the same cell (causing convergence issue)

###### (b) Predict the localization from the cell-center and allow keypoints to be outside the border for better matching and aggregation.

### **Descriptor Learning**
#### ^^Subpixel convolutions^^ via [[pixel-shuffle]] operations can greatly improve the quality of dense predictions, especially in [[Self-supervised]] regime

#### Before regressing, we first fast **upsample** to promote the capture of finer details in a higher resolution grid.

#### [[metric-learning]] is used to train by per-pixel [[Triplet loss]] instead of common [[Contrastive Loss]].
##### [[Nested hardest sample mining]]:: We pick the negative sample ^^closest^^ in the descriptor space but not a positive sample.

##### #recall: Each keypoint $$p_i\in \mathbf{p_s}$$ in source image has descriptor $$f_i$$, an ^^anchor^^ descriptor, obtained by sampling the appropriate location in the dense descriptor map $$\mathbf{f_s}$$ (in [[Superpoint]]).

##### This associated descriptor $$f_{i,+}^*$$ in the target frame, a __positive__ descriptor, is obtained by sampling the appropriate location in the target descriptor map $$\mathbf{f_t}$$ based on warped keypoint position $$p_i^*$$.

##### The nested [[Triplet loss]] is:
###### (3)    $$L_{desc}=\sum\limits_{i}\max{\left(0, ||\mathbf{f}_i, \mathbf{f}_{i,+}^*||_2-||\mathbf{f}_i, \mathbf{f}_{i,-}^*||_2+m\right)}$$ 

### **Score Learning**
#### Objective of $$L_{score}$$ is two-fold:
##### ensure the feature pairs have consistent scores

##### Network should learn that good keypoints are the ones with low feature point distance.
###### Minimize the squared distance between scores for each [[point-pair]]

###### Minimize/maximize the average score of a [[point-pair]] if the distance between paired keypoints is greater/less than average distance

#### (4)    $$L_{score}=\sum\limits_i \left[\frac{(s_i+\hat{s_i})}{2}\cdot \left(d(p_i,\hat{p_i})-\bar{d}\right)+(s_i-\hat{s_i})^2\right]$$
##### where $$s_i$$ and $$\hat{s_i}$$ are the source and target frame scores

##### $$\bar{d}=\sum\limits_i^L \frac{d(p_i, \hat{p_i})}{L}$$: average reprojection error of associated points in the current frame
###### $$d$$ feature distance in 2D Euclidean space

###### $$L$$ the total number of feature pairs

## 2. IO-NET:: Neural Outlier Rejection as an **Auxiliary** Task
### Use [[outlier rejection]] as a [[proxy task]] to supervise the keypoint learning.

### (5)    $$L_{IO}=\sum\limits_i \frac{1}{2}\left(r_i-\text{sign}{(||p_i^*-\hat{p_i}||_2-\epsilon_{uv})}\right)^2$$
where $$r$$ is the [[IO-Net]] output with $$\epsilon_{uv}$$ the same Euclidean distance threshold in last section.

### The final training objective:
#### (6)    $$\mathcal{L}=\alpha L_{loc}+\beta L_{desc} + \lambda L_{score} + L_{IO}$$