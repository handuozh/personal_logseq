---
title: deformable detr
tags: [[object detection]], paper, [[transformer]], [[Deformable Convolution Network]], [[attention]]
---
## Deformable DETR: Deformable Transformers for End-to-End Object Detection #toread

### Zotero Metadata

#### * Item Type: [[Article]]
#### Authors: [[Xizhou Zhu]], [[Weijie Su]], [[Lewei Lu]], [[Bin Li]], [[Xiaogang Wang]], [[Jifeng Dai]]
#### * Date: [[2020-10-08]]
#### [http://arxiv.org/abs/2010.04159](http://arxiv.org/abs/2010.04159)
#### Cite key: zhuDeformableDETRDeformable2020
#### Topics: [[Detection]]
#### Tags: #Computer-Science---Computer-Vision-and-Pattern-Recognition, #transformer, #deformable #zotero #literature-notes #reference

#### PDF Attachments
	- [Zhu et al_2020_Deformable DETR.pdf](zotero://open-pdf/library/items/663HLCQ7)

#### [[abstract]]:
DETR has been recently proposed to eliminate the need for many hand-designed components in object detection while demonstrating good performance. However, it suffers from slow convergence and limited feature spatial resolution, due to the limitation of Transformer attention modules in processing image feature maps. To mitigate these issues, we proposed Deformable DETR, whose attention modules only attend to a small set of key sampling points around a reference. Deformable DETR can achieve better performance than DETR (especially on small objects) with 10$\times$ less training epochs. Extensive experiments on the COCO benchmark demonstrate the effectiveness of our approach. Code shall be released.

#### zotero items: [Local library](zotero://select/items/1_J3K668QF)
## Backbone, matcher 和positional encoding的实现与[[DETR]]一样
### 主要修改在`deformable_detr.py`和`deformable_transformer.py`中
### 有效之处在于multi-scale deformable attention
## 相比于 [[DETR]]直接回归bounding box的绝对坐标,不依赖任何prior information
### 引入了reference,回归的是基于point坐标的offset
##