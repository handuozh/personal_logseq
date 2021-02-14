---
title: parallelized 3D tracking with GNN
pulic: true
type: Article
authors: [[Xinshuo Weng]], [[Ye Yuan]], [[Kris Kitani]]
citekey: wengParallelized3DTracking
#GNN #zotero #literature-notes #reference, paper, [[mot]], [[3d track]]
---

##
## Parallelized 3D Tracking and Forecasting with Graph Neural Networks and Diversity Sampling #readdone
### Zotero Metadata

#### PDF Attachments
	- [Weng et al. - Parallelized 3D Tracking and Forecasting with Grap.pdf](zotero://open-pdf/library/items/7X2SYIC6)

#### [[abstract]]:
Multi-object tracking (MOT) and trajectory forecasting are two critical components in modern 3D perception systems that require accurate modeling of multi-agent interaction. We hypothesize that it is beneﬁcial to unify both tasks under one framework in order to learn a shared feature representation of agent interaction. Furthermore, instead of performing tracking and forecasting sequentially which can propagate errors from tracking to forecasting, we propose a parallelized framework to mitigate the issue. Also, our proposed parallel track-forecast framework incorporates two additional novel computational units. First, we employ a feature interaction technique by introducing Graph Neural Networks (GNNs) to capture the way in which agents interact with one another. The GNN is able to improve discriminative feature learning for MOT association and provide socially-aware contexts for trajectory forecasting. Second, we use a diversity sampling function to improve the quality and diversity of our forecasted trajectories. The learned sampling function is trained to efﬁciently extract a variety of outcomes from a generative trajectory distribution and helps avoid the problem of generating duplicate trajectory samples. We evaluate on KITTI and nuScenes datasets showing that our method with socially-aware feature learning and diversity sampling achieves new state-of-the-art performance on both 3D MOT and trajectory forecasting.

#### zotero items: [Local library](zotero://select/items/1_YICQ8PIG)