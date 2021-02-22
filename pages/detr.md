---
title: DETR
type: Article
citekey: carionEndtoEndObjectDetection2020
publication date: [[2020-05-28]]
authors: [[Nicolas Carion]], [[Francisco Massa]], [[Gabriel Synnaeve]], [[Nicolas Usunier]], [[Alexander Kirillov]], [[Sergey Zagoruyko]]
tags: #attention, #detection, #transformer, #zotero, #literature-notes, #reference, [[object detection]]
---

## End-to-End Object Detection with Transformers #reading
### Zotero Metadata

#### [http://arxiv.org/abs/2005.12872](http://arxiv.org/abs/2005.12872)
#### code: https://github.com/facebookresearch/detr
#### PDF Attachments
	- [Carion et al_2020_End-to-End Object Detection with Transformers.pdf](zotero://open-pdf/library/items/LJKH5VA3)

#### [[abstract]]:
##### We present a new method that views object detection as a **direct set prediction problem**.
##### Our approach streamlines the detection pipeline, effectively removing the need for many hand-designed components that explicitly encode our prior knowledge about the task.
###### 取代现在模型需要手工设计的工作
####### [[NMS]]
####### anchor generation
##### The main ingredients of the new framework, called DEtection TRansformer or DETR, are a ^^set-based global loss^^ that forces unique predictions via [[bipartite graph matching]], and a _transformer encoder-decoder architecture_.
###### Given a fixed small set of learned object queries, DETR reasons about the relations of the objects and the **global image context** to directly output the final set of predictions in parallel.
###### The new model is conceptually simple and does not require a specialized library, unlike many other modern detectors.
##### DETR demonstrates accuracy and run-time performance on par with the well-established and highly-optimized [[Faster R-CNN]] baseline on the challenging [[COCO]] object detection dataset.
###### Moreover, DETR can be easily generalized to produce panoptic segmentation in a unified manner.
###### We show that it significantly outperforms competitive baselines.
#### zotero items: [Local library](zotero://select/items/1_RJB6MLHT)
## Overview
:PROPERTIES:
:heading: true
:END:
### 两个关键点
#### [[transformer]]的 [[Encoder-decoder]]架构一次性生成$N$个(100)box prediction
#### 设计 [[bipartite graph matching]] loss
##### 基于predicted boxes和gt boxes进行bipartite二分图匹配计算loss
### ![image.png](../assets/pages_detr_1613963899263_0.png)
## 1. DETR Architecture
:PROPERTIES:
:heading: true
:END:
### 三个部分: encoder, decoder, FFN
### ![image.png](../assets/pages_detr_1613966635630_0.png)
#### CNN backbone to learn a 2D representation.
##### flattened and supplemented with a positional encoding before entering encoder
#### A transformer encoder takes a small fixed number of learned positional embeddings
##### **object queries**
#### pass the output embedding of decoder to a ^^shared^^ [[feed forward network]] (FFN)
##### to predict either a detection or "no object" class
###
