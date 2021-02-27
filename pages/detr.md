---
title: DETR
type: Article
citekey: carionEndtoEndObjectDetection2020
publication date: [[2020-05-28]]
authors: [[Nicolas Carion]], [[Francisco Massa]], [[Gabriel Synnaeve]], [[Nicolas Usunier]], [[Alexander Kirillov]], [[Sergey Zagoruyko]]
tags: #attention, #detection, #transformer, #zotero, #literature-notes, #reference, [[object detection]]
---

## End-to-End Object Detection with Transformers #readdone
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
####### 不需要设置先验anchor,几乎没有超参数,也不需要NMS(因为输出的无需集合没有重复情况)
##### The main ingredients of the new framework, called DEtection TRansformer or DETR, are a ^^set-based global loss^^ that forces unique predictions via [[bipartite graph matching]], and a _transformer encoder-decoder architecture_.
###### Given a fixed small set of learned object queries, DETR reasons about the relations of the objects and the **global image context** to directly output the final set of predictions in parallel.
####### infer固定数量(100)的预测集合 #practical
######## 每个集合包括类别和坐标信息
######### $y_i=(c_i;b_i)$
######### $c$ 是92(91+1)个类别响亮
######### $b$ 是长度为4的bbox坐标向量
######## 输出的时候就会有两个分支: bbox分支(b,100,4)和分类分支(b,100,92)
####### 图中物体不够$N$个就用 $\phi$ (no object) 填充padding
######## 计算loss的时候bbox分支仅计算有物体位置,背景集合忽略
###### The new model is conceptually simple and does not require a specialized library, unlike many other modern detectors.
##### DETR demonstrates accuracy and run-time performance on par with the well-established and highly-optimized [[Faster R-CNN]] baseline on the challenging [[COCO]] object detection dataset.
###### Moreover, DETR can be easily generalized to produce [[panoptic segmentation]] in a unified manner.
###### We show that it significantly outperforms competitive baselines.
#### zotero items: [Local library](zotero://select/items/1_RJB6MLHT)
## Overview
:PROPERTIES:
:heading: true
:END:
### 两个关键点
#### [[transformer]]的 [[Encoder-decoder]]架构一次性生成$N$个(100)box prediction
##### 有几处针对目标检测对原始 [[transformer]] 进行的网络结构改进
#### 设计 [[bipartite graph matching]] loss
##### 基于predicted boxes和gt boxes进行bipartite二分图匹配计算loss
### ![image.png](../assets/pages_detr_1613963899263_0.png){:height 143, :width 749}
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
:PROPERTIES:
:heading: true
:END:
#### $\mathbf{x}_{img}\in{\mathbb{R}^{3\times H_0 \times W_0}}$ -> feature map $f\in \mathbb{R}^{C\times H \times W}$
##### $C=2048$
##### $H, W=\frac{H_0}{32}, \frac{W_0}{32}$
### 1.2 encoder
:PROPERTIES:
:heading: true
:END:
#### a $1\times 1$ convolution reduces the channel dim of the high-level activation map $f$
##### $C$ to a smaller dimension $d$
###### 维度压缩
###### new feature map $z_0\in \mathbb{R}^{d\times H \times W}$
##### reshape -> $d\times HW$ feature map
###### 序列化数据
#### each encoder layer has a **multi-head self-attention** module and a [[FFN]]
#### 因为transformer encoder结构是permutation-invariant的 (顺序无关的)
##### 每一个attention layer的输入加上fixed [[positional encoding]]s
###### 反映位置信息
###### 考虑2D空间,即xy方向
###### 每个方向128维
####
#+BEGIN_NOTE
位置编码向量仅加到Q和K中,$V$没有加入位置信息
和原始做法不同
#+END_NOTE
####
#+BEGIN_NOTE
位置编码向量加入到每个编码器(6个)中
原始做法只有第一个编码器输入位置编码向量
#+END_NOTE
### 1.3 Decoder
:PROPERTIES:
:heading: true
:END:
#### 跟标准transformer区别在于
##### the model decodes the $N$ objects **in parallel** at each decoder layer
#### 因为transformer decoder结构是permutation-invariant的
##### $N$个input embeddings must be different to produce different results
###### shape $(100, 256)$
###### 被称为^^object queries^^
####### 在学习过程中提供target和context image 之间的关系,相当于全局注意力
######## 通俗理解: object queries矩阵内部通过学习建模了100个物体之间的全局关系，例如房间里面的桌子旁边(A类)一般是放椅子(B类)，而不会是放一头大象(C类)
######## 训练过程中每个格子(共N个)的向量都会包括整个训练集相关的位置和类别信息
######### 例如第0个格子里存储某个空间位置的大象类别的嵌入向量
######### 但是这个大象类别embedding vector和某一张图片的大象特征无关,而是通过训练考虑了所有图片里的某个位置附近的大象编码特征
######### 属于和位置有关的全局大象统计信息
########
#+BEGIN_SRC python
# num_queries=100,hidden_dim=256
self.query_embed = nn.Embedding(num_queries, hidden_dim)
#+END_SRC
######## 训练过程
######### 图片输入到encoder中进行特征编码,输出的编码向量就是$K$和$V$
######### **object queries**是$Q$
######### 计算QQ和KK,加权VV得到编码器的输出
########## 第0个格子的$q_0$会和KK中所有向量计算
########## 目的是查找某个位置附近有没有大象
########## 如果有会加权输出
######### 整个过程计算后就可有把编码vector提取出来,特征已经对齐了
######## 作用有点类似anchor in [[Faster R-CNN]],但是是可学习的,不是提前设置好的
########
###### 被decoder转换成output embeddings
#### Using self and encoder-decoder attention over these embeddings, the model globally reasons about all objects together using [[Pairwise]] relations between them
##### while able to use the whole image as context
#### 一次解码输出全部unordered set
##### 因为任务是无序集合，做成顺序推理有序输出没有必要
##### 只需要初始化时候输入一个全0的查询向量A，类似于BOS_WORD作用
###### 然后第一个解码器接受该输入A，解码输出向量作为下一个解码器输入，不断推理即可.
###### 不需要在第二个解码器输入时候考虑BOS_WORD和第一个解码器输出.
### 1.4 **Feed Forward Network**
{{embed ((60389635-8602-43a5-9b4d-dc077719605a)) }}
### 1.5 Auxiliary decoding losses
:PROPERTIES:
:heading: true
:END:
#### add prediction FFNs and [[Hungarian]] loss after each decoder layer
##### 作者实验发现，如果对解码器的每个输出都加入辅助的分类和回归loss，可以提升性能
##### 作者除了对最后一个编码层的输出进行Loss监督外，还对其余5个编码器采用了**同样的loss监督**，只不过权重设置低一点而已
#### All FFNs share the parameters
#### Use additional shared **layer-norm** to normalize input to the prediction FFNs from different decoder layers
### ![image.png](../assets/pages_detr_1613987449992_0.png)
## 2. Loss
:PROPERTIES:
:heading: true
:END:
### 2.1 Object detection set prediction loss to force **unique** matching
:PROPERTIES:
:heading: true
:END:
#### 为什么要匹配? #motivation
##### 因为输出的$b\times 100$个检测集合是无序的,如何和**gt** bbox计算loss呢?
##### 需要 [[bipartite graph matching]] 双边匹配得到匹配索引 index
#### ground truth boxes的个数(即图中object的个数)为$m$，由于$N$是一个事先设定好的远远大于image objects个数的整数，所以$N>>m$即生成的prediction boxes的数量会远远大于ground truth boxes 数量
##### 为了解决上述问题,认为构造一个新的物体类别 $\phi$ 表示没有物体
##### 多出来的$N-m$个prediction embedding就会和 $\phi$类别配对
#### (1) 用 [[Hungarian]] 找到cost最小的 bipartite matching方案
##### search for a permutation of $N$ elements $\sigma \in \mathcal{G}_N$ with the lowest cost
###### $\hat{\sigma}(i)$表示无序gt bbox集合的哪个元素和输出预测集合中的第$i$个匹配
######
$$
\hat{\sigma}=\underset{\sigma \in \mathfrak{S}_{N}}{\arg \min } \sum_{i}^{N} \mathcal{L}_{\operatorname{match}}\left(y_{i}, \hat{y}_{\sigma(i)}\right)
$$
###### $y$ denotes **gt** set of objects (size $N$ padded with $\phi$)
###### $\hat{\mathbf{y}}=\{\hat{y}_i\}^N_{i=1}$ the set of $N$ predictions
###### $\mathcal{L}_{\operatorname{match}}\left(y_{i}, \hat{y}_{\sigma(i)}\right)$ [[Pairwise]] matching cost between **gt** $y_i$ and prediction with index $\sigma(i)$
##### 这个matching cost 考虑class prediction 和 similarity
######
$$
\mathcal{L}_{\operatorname{match}}\left(y_{i}, \hat{y}_{\sigma(i)}\right) = -\mathbf{1}_{\left\{c_{i} \neq \varnothing\right\}} \hat{p}_{\sigma(i)}\left(c_{i}\right)+\mathbf{1}_{\left\{c_{i} \neq \varnothing\right\}} \mathcal{L}_{\mathrm{box}}\left(b_{i}, \hat{b}_{\sigma(i)}\right)
$$
###### $c_i$是target class label (might be $\phi$)
###### $b_i \in \left[0,1\right]^4$是vector to define gt box center coordinates and height, width w.r.t image size
###### $\hat{p}_{\sigma(i)}(c_i)$是probability of class $c_i$
###### $\hat{b}_{\sigma(i)}$是predicted box
##### 重点是找到one-to-one 的匹配,没有duplicates
#### (2) 计算loss function
##### The _Hungarian loss_ for all pairs matched in step (1)
##### 跟常规object detectors类似
###### linear combination of a negative log-likelihood
####### class prediction
####### box loss
###### 提供输入的$N$个输出集合和$M$个gt bbox之间的**关联程度**
#######
$$
\mathcal{L}_{\text {Hungarian }}(y, \hat{y})=\sum_{i=1}^{N}\left[-\log \hat{p}_{\hat{\sigma}(i)}\left(c_{i}\right)+\mathbf{1}_{\left\{c_{i} \neq \varnothing\right\}} \mathcal{L}_{\text {box }}\left(b_{i}, \hat{b}_{\hat{\sigma}}(i)\right)\right]
$$
####### $\hat{\sigma}$ optimal assignment in step (1)
####### down-weight the log-probability term when $c_i=\phi$ by factor 10 #practical
######## 解决 class imbalance
######## #related [[Faster R-CNN]] training procedure balances positive/negative by ^^subsampling^^
###### The matching cost between object and $\phi$ does not depend on prediction
####### in this case the cost is constant
##### Bounding box loss
###### make box predictions directly
####### 不同与其他detectors w.r.t. initial guesses
###### 为了解决scale的问题
####### linear **combination** of the [[l1 loss]] and the generalized [[IoU]] loss $\mathcal{L}_{iou}$
######## GIOU is scale-invariant
###### $\mathcal{L}_{box}(b_i, \hat{b}_{\sigma(i)})$ defined as:
#######
$$
\lambda_{\text {iou }} \mathcal{L}_{\text {iou }}\left(b_{i}, \hat{b}_{\sigma(i)}\right)+\lambda_{\mathrm{L} 1}\left\|b_{i}-\hat{b}_{\sigma(i)}\right\|_{1}
$$
######## $\left\|b_{i}-\hat{b}_{\sigma(i)}\right\|$ 为两个box中心坐标的 [[l1 distance]]
######
###
