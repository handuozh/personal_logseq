---
title: CenterTrack
type: Article
citekey: zhouTrackingObjectsPoints2020
publication date: [[2020]]
authors: [[Xingyi Zhou]], [[Vladlen Koltun]], [[Philipp Krähenbühl]]
tags: #centernet #zotero #literature-notes #reference #MOT
---
## Tracking Objects as Points #toread
### Zotero Metadata

#### [http://link.springer.com/10.1007/978-3-030-58548-8_28](http://link.springer.com/10.1007/978-3-030-58548-8_28)

#### PDF Attachments
	- [Zhou et al. - 2020 - Tracking Objects as Points.pdf](zotero://open-pdf/library/items/QF2LTQTZ)

#### [[abstract]]:
##### Tracking has traditionally been the art of following interest points through space and time. This changed with the rise of powerful deep networks.
###### Nowadays, tracking is dominated by pipelines that perform object detection followed by temporal association, also known as [[tracking-by-detection]].
###### We present a simultaneous detection and tracking algorithm that is simpler, faster, and more accurate than the state of the art.
##### Our tracker, **CenterTrack**, applies a detection model to a pair of images and detections from the prior frame.
###### Given this minimal input, **CenterTrack** localizes objects and predicts their associations with the previous frame.
####### CenterTrack is simple, online (no peeking into the future), and real-time.
##### It achieves 67.8% MOTA on the **MOT17** challenge at 22 FPS and 89.4% MOTA on the [[KITTI]] tracking benchmark at 15 FPS, setting a new state of the art on both datasets.
##### CenterTrack is easily extended to monocular 3D tracking by regressing additional 3D attributes. Using monocular video input, it achieves 28.3% AMOTA@0.2 on the newly released [[nuScenes]] 3D tracking benchmark, substantially outperforming the monocular baseline on this benchmark while running at 28 FPS.
#### zotero items: [Local library](zotero://select/items/1_G9HIGQCK)t
##
