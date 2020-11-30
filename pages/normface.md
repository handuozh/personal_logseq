---
title: NormFace
---

## Abstract
### 在一个人脸识别任务中，特征的归一化是一个提升性能的重要步骤。本文主要研究训练过程中的特征归一化的影响
### 通过数学分析研究了四个与归一化有关的问题，可以帮助理解设置参数。基于这种分析并提出了采用归一化特征训练的两种策略:
#### softmax损失的修改，优化余弦相似度代替优化内积
#### 通过为每个类引入一个引导向量重新制定度量学习
## Pipeline
### ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FSLAM%2FrkSKf8gepa.png?alt=media&token=fa37c4ae-7e28-440c-adfa-a07deb200558)
## Normalization
### ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FSLAM%2FCp7FQXFUoy.png?alt=media&token=675156b6-7526-444b-a1e4-8d0fe0c8943c)
##
