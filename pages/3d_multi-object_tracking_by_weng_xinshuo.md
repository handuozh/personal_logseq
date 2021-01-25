---
title: 3D multi-object tracking by Weng Xinshuo
tags: paper, [[mot]], [[3d track]]
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
### ![image.png](/assets/pages_3d_multi-object_tracking_by_weng_xinshuo_1611217315255_0.png){:height 207, :width 641}
### (A) [[LiDAR]] point cloud -> 3D detections $D_t$
### (B) 3D [[Kalman Filter]] predicts the state of trajectories $T_{t-1}$ to current frame $t$ as $T_{est}$
### (C) the detections $D_t$ and predicted trajectories $T_{est}$ are associated using the [[Hungarian]] algorithm
### (D) the state of each matched trajectory in $T_{match}$ is updated by the 3D [[Kalman Filter]]
#### based on the corresponding matched detection in $D_{match}$ to obtain the final trajectories $T_t$
### (E)
##