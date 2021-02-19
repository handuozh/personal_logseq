---
title: TransTrack
type: Article
citekey: sunTransTrackMultipleObjectTracking2020
publication date: [[2020-12-31]]
authors: [[Peize Sun]], [[Yi Jiang]], [[Rufeng Zhang]], [[Enze Xie]], [[Jinkun Cao]], [[Xinting Hu]], [[Tao Kong]], [[Zehuan Yuan]], [[Changhu Wang]], [[Ping Luo]]
tags: [[MOT]], #zotero, #literature-notes, #reference, [[transformer]], [[query-key]]
---
## TransTrack: Multiple-Object Tracking with Transformer #toread
### Zotero Metadata
#### #code  https://github.com/PeizeSun/TransTrack
#### apper: [http://arxiv.org/abs/2012.15460](http://arxiv.org/abs/2012.15460)
#### PDF Attachments
	- [Sun et al_2020_TransTrack.pdf](zotero://open-pdf/library/items/2YJ93IDV)

#### [[abstract]]:
##### Multiple-object tracking(MOT) is mostly dominated by complex and multi-step [[tracking-by-detection]] algorithm, which performs object detection, feature extraction, and temporal association, separately.
##### ^^query-key^^ mechanism in single-object tracking(SOT), which tracks the object of the current frame by object feature of the previous frame, has great potential to set up a simple _joint-detection-and-tracking_ MOT paradigm.
###### Nonetheless, the query-key method is seldom studied due to its inability to detect new-coming objects.
##### In this work, we propose **TransTrack**, a baseline for MOT with Transformer.
###### It takes advantage of ^^query-key^^ mechanism and introduces a set of learned object queries into the pipeline to enable detecting new-coming objects.
##### **TransTrack** has 3 main advantages:
###### (1) It is an online joint-detection-and-tracking pipeline based on query-key mechanism. Complex and multi-step components in the previous methods are simplified.
###### (2) It is a brand new architecture based on _Transformer_. The learned object query detects objects in the current frame. The object feature query from the previous frame associates those current objects with the previous ones.
###### (3) For the first time, we demonstrate a much simple and effective method based on query-key mechanism and Transformer architecture could achieve competitive 65.8\% MOTA on the MOT17 challenge dataset.
#### zotero items: [Local library](zotero://select/items/1_H36YUSUS)
## Pipeline
### ![image.png](../assets/pages_transtrack_1613636460588_0.png)
###
