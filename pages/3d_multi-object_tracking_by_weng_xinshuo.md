---
title: 3D multi-object tracking by Weng Xinshuo
---

## Meta Data
:PROPERTIES:
:heading: true
:END:
### ## Parallelized 3D Tracking and Forecasting with Graph Neural Networks and Diversity Sampling #toread

### Zotero Metadata

#### * Item Type: [[Article]]
#### * Authors: [[Xinshuo Weng]], [[Ye Yuan]], [[Kris Kitani]]
#### * Cite key: wengParallelized3DTracking
#### * Topics: [[3D]]
#### * Tags: #GNN #zotero #literature-notes #reference

#### PDF Attachments
	- [Weng et al. - Parallelized 3D Tracking and Forecasting with Grap.pdf](zotero://open-pdf/library/items/7X2SYIC6)

#### [[abstract]]:
Multi-object tracking (MOT) and trajectory forecasting are two critical components in modern 3D perception systems that require accurate modeling of multi-agent interaction. We hypothesize that it is beneﬁcial to unify both tasks under one framework in order to learn a shared feature representation of agent interaction. Furthermore, instead of performing tracking and forecasting sequentially which can propagate errors from tracking to forecasting, we propose a parallelized framework to mitigate the issue. Also, our proposed parallel track-forecast framework incorporates two additional novel computational units. First, we employ a feature interaction technique by introducing Graph Neural Networks (GNNs) to capture the way in which agents interact with one another. The GNN is able to improve discriminative feature learning for MOT association and provide socially-aware contexts for trajectory forecasting. Second, we use a diversity sampling function to improve the quality and diversity of our forecasted trajectories. The learned sampling function is trained to efﬁciently extract a variety of outcomes from a generative trajectory distribution and helps avoid the problem of generating duplicate trajectory samples. We evaluate on KITTI and nuScenes datasets showing that our method with socially-aware feature learning and diversity sampling achieves new state-of-the-art performance on both 3D MOT and trajectory forecasting.

#### zotero items: [Local library](zotero://select/items/1_YICQ8PIG)
## Based on the detection of [[Point-RCNN]] which is lidar based
## System Pipeline
:PROPERTIES:
:heading: true
:END:
### ![image.png](/assets/pages_3d_multi-object_tracking_by_weng_xinshuo_1611217315255_0.png){:height 194, :width 718}
### (A) [[LiDAR]] point cloud -> 3D detections $D_t$
#### $D_t=\{D_t^1,D_t^2,\cdots,D_t^{n_t}\}$ ($n_t$ is detection numbers)
#### Each detection $D_t^j$ where $j\in{\{1,2,\cdots, n_t\}}$ is represented as
##### a tuple $(x,y,z,\theta,l,w,h,s)$
###### location of object center in 3D space $(x,y,z)$
###### object 3D size $(l,w,h)$
###### heading angle $\theta$
###### confidence score $s$
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
###### by computing 3D [[intersection over union]] or ^^negative center distance^^([[?]]) between every pair of the trajectory $T_{est}^i$ and
### (D) the state of each matched trajectory in $T_{match}$ is updated by the 3D [[Kalman Filter]]
#### based on the corresponding matched detection in $D_{match}$ to obtain the final trajectories $T_t$
### (E) Birth and death memory
#### (1) Consider all unmatched detections $D_{\rm{unmatch}}$ as potential new objects entering the scene.
##### Might be false positive, do not create new trajectory $T_{new}^P$ until $D_{\rm{unmatch}}$ has been **continually** matched in the next $\rm{Bir_{min}}$ frames
######
##
