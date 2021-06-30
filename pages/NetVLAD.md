---
title: NetVLAD
type: Article
public: true
authors: [[Relja Arandjelović]], [[Petr Gronat]], [[Akihiko Torii]], [[Tomas Pajdla]], [[Josef Sivic]]
citekey: arandjelovicNetVLADCNNArchitecture2016
publication date: [[2016-05-02]] 
proceeding: [[CVPR]] 
tags: [[VLAD]] , [[Image retrieval]], [[reference]], [[literature-notes]] , [[zotero]] 
---

- Meta Data
  heading:: true
    - #title NetVLAD: CNN architecture for weakly supervised place recognition, #reading
        - #url:  [http://arxiv.org/abs/1511.07247](http://arxiv.org/abs/1511.07247)
        - PDF Attachments
            - [2016_Arandjeloqvic et al_arXiv1511.07247 [cs]_NetVLAD.pdf](zotero://open-pdf/library/items/3UMR8UA8)
        - zotero items: [Local library](zotero://select/items/1_PCHUWL4Y)
        - [[abstract]]
            - We tackle the problem of large scale visual place recognition, where the task is to quickly and accurately recognize the location of a given query photograph. We present the following three principal contributions. First, we develop a convolutional neural network (CNN) architecture that is trainable in an end-to-end manner directly for the place recognition task. The main component of this architecture, NetVLAD, is a new generalized VLAD layer, inspired by the “Vector of Locally Aggregated Descriptors” image representation commonly used in image retrieval. The layer is readily pluggable into any CNN architecture and amenable to training via backpropagation. Second, we develop a training procedure, based on a new weakly supervised ranking loss, to learn parameters of the architecture in an end-to-end manner from images depicting the same places over time downloaded from Google Street View Time Machine. Finally, we show that the proposed architecture signiﬁcantly outperforms non-learnt image representations and off-the-shelf CNN descriptors on two challenging place recognition benchmarks, and improves over current stateof-the-art compact image representations on standard image retrieval benchmarks.
    - #topic #[[Image retrieval]]
- Abstract
  heading:: true
    - 直接在可训练的CNN架构中嵌入VLAD层是不可行的，因为VLAD中的$$\alpha_k(x_i)$$是不连续的，因此论文作者对 $$\alpha_k(x_i)$$做了修改，提出了NetVLAD.
    - 训练一个模型，对于每一张输入的图像$$I_i$$，得到一个固定长度的向量$$f(I_i)$$。依此法对所要查询的图像q进行操作， 也得到了q的向量表达$$f(q)$$，然后依次与数据集进行比对即可。
    - 怎么衡量两个向量相近呢？这篇论文用的是欧氏距离(Euclidean distance)
    - 1. 为场景识别任务构造出了一个可以直接端到端训练的CNN模型结构，NetVLAD就是该模型的一个layer
    - 2. 构造一个弱监督排序损失([[weakly supervised]] [[Ranking]] loss)来指导模型的参数更新
    - 3. 效果很好。在两个具有挑战性的数据集上超过了非学习性的和现成的CNN描述子，等等．
- NetVLAD
  heading:: true
    - 把[[VLAD]]中的指示函数$$\alpha_k(x_i)$$换成可导的$$\bar{\alpha}_k(x_i)$$
        - $$\bar{\alpha_k}(x_i)=\frac{e^{w_k^{top}x_i+b_k}}{\sum_{k'}e^{w_{k'}^{top}x_i+b_{k'}}}$$
        - $$\bar{\alpha_k}(x_i)$$表示把$$x_i$$分配给聚类中心$$c_k$$的权重，取值范围为0-1之间．
        - $$\bar{\alpha_k}(x_i)$$越大表示$$x_i$$与$$c_k$$的距离越近．
        - $$\alpha$$是一个常数，当$$\alpha\rightarrow +\infty$$时，$$\bar{\alpha_k}(x_i)$$取值无限接近于１或０了．
    - 最后得到$$V(j,k)$$的表达式
        - $$V(j,k)=\sum\limits_{i=1}^N =\frac{e^{w_k^{top}x_i+b_k}}{\sum_{k'}e^{w_{k'}^{top}x_i+b_{k'}}}\left(x_i(j)-c_k(j)\right)$$
        - 其中$$w_k$$,$$b_k$$,$$c_k$$均为可学习的参数．
- Architecture
  heading:: true
    - ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FSLAM%2F7rYkbYZqhN.png?alt=media&token=3fd69688-d5c6-4ea9-8d8a-2c7a4902658b)
    - TODO  Read on!
      todo:: 1606462977225
- Supervsed VLAD
    - ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FSLAM%2Fwai5aWdSY7.png?alt=media&token=5fd74c67-6b94-4a8a-b955-995321e690e7)
    -