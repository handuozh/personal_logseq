---
title: Deformable Convolution Network(DCN)
---

## #paper
### Deformable convolutional networks, ICCV 2017 (DCN V1)
### Deformable ConvNets v2: More Deformable, Better Results
## 可变卷积和可变ROI采样
## 就是在这些卷积或者ROI采样层上，添加了**位移变量**，这个变量根据数据的情况学习
### 偏移后，相当于卷积核每个方块可伸缩的变化，从而改变了感受野的范围，感受野成了一个多边形
## [deformable conv](https://i.imgur.com/ABfMmYo.png)
### 上图是在二维平面上deformable convolution和普通的convolution的描述图
#### (a)是普通的卷积，卷积核大小为3*3，采样点排列非常规则，是一个正方形
#### (b)是可变形的卷积，给每个采样点加一个offset
##### 这个offset通过额外的卷积层学习得到），排列变得不规则
#### (c)和(d)是可变形卷积的两种特例
##### 对于(c)加上offset，达到尺度变换的效果aspect ratio
##### 对于(d)加上offset，达到旋转变换的效果rotation
##
