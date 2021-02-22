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
### 1.1 Backbone
#### $\mathbf{x}_{img}\in{\mathbb{R}^{3\times H_0 \times W_0}}$ -> feature map $f\in \mathbb{R}^{C\times H \times W}$
##### $C=2048$
##### $H, W=\frac{H_0}{32}, \frac{W_0}{32}$
### 1.2 encoder
:PROPERTIES:
:heading: true
:END:
#### a $1\times 1$ convolution reduces the channel dim of the high-level activation map $f$
##### $C$ to a smaller dimension $d$
##### new feature map $z_0\in \mathbb{R}^{d\times H \times W}$
##### -> $d\times HW$ feature map
#### each encoder layer has a **multi-head self-attention** module and a [[FFN]]
#### 因为transformer encoder结构是permutation-invariant的
##### 每一个attention layer的输入加上fixed [[positional encoding]]s
### 1.3 Decoder
:PROPERTIES:
:heading: true
:END:
#### 跟标准transformer区别在于
##### the model decodes the $N$ objects in parallel at each decoder layer
#### 因为transformer decoder结构是permutation-invariant的
##### $N$个input embeddings must be different to produce different results
###### 被成为^^object queries^^
###### 被decoder转换成output embeddings
###
