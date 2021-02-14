---
title: 3D multi-object tracking by Weng Xinshuo
tags: #zotero #literature-notes #reference, paper, [[mot]], [[3d track]], [[Kalman Filter]]
public: true
type: Article
authors: [[Xinshuo Weng]], [[Jianren Wang]], [[David Held]], [[Kris Kitani]]
citekey: weng3DMultiObjectTracking
---
## Meta Data
:PROPERTIES:
:heading: true
:END:
### Title: 3D Multi-Object Tracking: A Baseline and New Evaluation Metrics
### PDF Attachments
	- [Weng et al_3D Multi-Object Tracking.pdf](zotero://open-pdf/library/items/624Z3NIL)
### [[abstract]]:
#### 近年3d [[MOT]] focuses on accuracy
##### less attention to 计算量和system complexity
#### Based on the detection of [[Point-RCNN]] which is lidar based
#### Online [[MOT]] system, data association
##### 结合3D [[Kalman Filter]] for state estimation
##### [[Hungarian]] algorithm for data association
#### 3 new [[Metrics]] to evaluate 3D MOT methods
### zotero items: [Local library](zotero://select/items/1_N2FSRQWZ)
## System Pipeline
:PROPERTIES:
:heading: true
:END:
### ![image.png](/assets/pages_3d_multi-object_tracking_by_weng_xinshuo_1611217315255_0.png){:height 194, :width 718}
### (A) 输入 [[LiDAR]] point cloud -> 3D detections $D_t$
#### $D_t=\{D_t^1,D_t^2,\cdots,D_t^{n_t}\}$ ($n_t$ is detection numbers)
#### Each detection $D_t^j$ where $j\in{\{1,2,\cdots, n_t\}}$ is represented as
##### a tuple $(x,y,z,\theta,l,w,h,s)$
###### location of object center in 3D space $(x,y,z)$
###### object 3D size $(l,w,h)$
###### heading angle $\theta$
###### confidence score $s$
##### 这种方式没有考虑3D object detection的
### (B) 3D [[Kalman Filter]] predicts the state of trajectories $T_{t-1}$ to current frame $t$ as $T_{est}$
#### 假设object inter-frame displacement using **constant velocity model**
##### independent of camera ego-motion
#### state of an object trajectory as a 11-dimensional vector $T=(x,y,z,\theta,l,w,h,s,v_x,v_y,v_z)$
##### No angular velocity $v_{\theta}$ in the state space for simplicity
###### 从经验来看,物体的角速度没有什么影响
#### 基于constant velocity model, state of associated trajectories $\mathbf{T}_{t-1}=\{T_{t-1}^1, \cdots, T_{t-1}^{m_t}\}$ propogated to frame $t$ as $T_{est}$
#####
$$x_{est}=x+v_x, /; /; y_{est}=y+v_y, /; /; z_{est}=z+v_z$$
### (C) Data Association
#### the detections $D_t$ and predicted trajectories $T_{est}$ are associated using the [[Hungarian]] algorithm
##### Construct affinity matrix $m_{t-1}\times n_t$
###### by computing 3D [[intersection over union]] or ^^negative center distance^^([[?]]) between every pair of the trajectory $T_{est}^i$ and detection $D_t^j$
###### [[bipartite graph matching]] problem solved by [[Hungarian]]
####### reject low 3D [[IoU]] less than a threshold
####### output $T_{match}, D_{match}$.
####### $T_{unmatch}$ , $D_{unmatch}$ are complementary set
### (D) the state of each matched trajectory in $T_{match}$ is updated by the 3D [[Kalman Filter]]
#### based on the corresponding matched detection in $D_{match}$ to obtain the final trajectories $T_t$
#### Following [[Bayes Rule]], the updated state of each trajectory is the weighted average between the state of $T_{match}^k$ and $D_{match}^k$.
##### Weights are determined by the state uncertainty
##### orientation $\theta$ might have ambiguity -> ^^orientation correction^^
###### make sure $D_{match}^k$ and $T_{match}^k$ are roughly consistent in orientation
### (E) Birth and death memory
#### (1) Consider all unmatched detections $D_{\rm{unmatch}}$ as potential new objects entering the scene.
##### Might be false positive, do not create new trajectory $T_{new}^P$ until $D_{\rm{unmatch}}$ has been **continually** matched in the next $\rm{Bir_{min}}$ frames
###### $p\in{\{1,2,\cdots,n_t - w_t\}}$
#### (2) Consider all unmatched trajectories $T_{unmatch}$ as potential objects leaving the scene.
##### Might delete true positive ones, keep tracking each $T_{unmatch}^q$ for $\rm{Age_{max}}$ frames before mark it as $T_{lost}^q$
###### $q\in{\{1,2, \cdots, m_{t-1}-w_t\}}$
##
##
##
