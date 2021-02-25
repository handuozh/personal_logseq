---
title: Objects as points
alias: [[centernet]]
type: Article
citekey: zhouObjectsPoints2019
publication date: [[2019-04-16]]
authors: [[Xingyi Zhou]], [[Dequan Wang]], [[Philipp Krähenbühl]]
tags: [[object detection]], #single-stage, #literature-notes, #reference, #Keypoint, #[[end-to-end]]
---
## Zotero Metadata #toread
:PROPERTIES:
:heading: true
:END:
### Objects as Points
### [http://arxiv.org/abs/1904.07850](http://arxiv.org/abs/1904.07850)
### PDF Attachments
	- [Zhou et al_2019_Objects as Points.pdf](zotero://open-pdf/library/items/EF6CK3WB)

### [[abstract]]:
Detection identiﬁes objects as axis-aligned boxes in an image. Most successful object detectors enumerate a nearly exhaustive list of potential object locations and classify each. This is wasteful, inefﬁcient, and requires additional post-processing.
### We model an object as a **single point** — the center point of its bounding box.
#### Our detector uses keypoint estimation to ﬁnd center points and regresses to all other object properties, such as size, 3D location, orientation, and even pose.
#### 直接预测bbox的中心点和尺寸
### Our center point-based approach, CenterNet, is end-to-end differentiable, simpler, faster, and more accurate than corresponding bounding box-based detectors.
#### CenterNet achieves the best speed-accuracy trade-off on the MS COCO dataset, with 28.1% AP at 142 FPS, 37.4% AP at 52 FPS, and 45.1% AP with multi-scale testing at 1.4 FPS. We use the same approach to estimate 3D bounding box in the KITTI benchmark and human pose on the COCO keypoint dataset. Our method performs competitively with sophisticated multi-stage methods and runs in real-time.
### zotero items: [Local library](zotero://select/items/1_2UB3K7BV)
## 1. Preliminary
### ((60361390-93a2-4b9e-b2e6-d932868d45b5))
### Output keypoint heatmap $\hat{Y}\in [0,1]^{\frac{W}{R}\times \frac{H}{R} \times C}$
#### $R$ is [[Stride]]
##### downsample the output prediction by a factor of $R$
##### set 4 here
#### $C$ the number of keypoint types
#### 使用$Y_{x,y,c}$表示heatmap中的第$c$个通道位置$(x,y)$处的值
##### $0$代表background
##### $1$代表detected keypoint
###### 所在位置为目标bbox的中心
### 若input image位置
