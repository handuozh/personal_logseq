---
title: Deformable Convolution Network
public: true
---
## #paper
### Deformable convolutional networks, ICCV 2017 (DCN V1)
### Deformable ConvNets v2: More Deformable, Better Results
### 同样的物体在图像中可能呈现出不同的大小、姿态、视角变化甚至非刚体形变, 如何适应这些复杂的几何形变是物体识别的主要难点
## Deformable Convolution
### 就是在这些卷积或者ROI采样层上，添加了**位移变量**，这个变量根据数据的情况学习
#### 偏移后，相当于卷积核每个方块可伸缩的变化，从而改变了感受野的范围，感受野成了一个多边形
### [deformable conv](https://i.imgur.com/ABfMmYo.png)
#### 上图是在二维平面上deformable convolution和普通的convolution的描述图
##### (a)是普通的卷积，卷积核大小为$3\times 3$，采样点排列非常规则，是一个正方形
##### (b)是可变形的卷积，给每个采样点加一个augmented offset
###### 这个offset通过额外的卷积层学习得到，排列变得不规则
##### (c)和(d)是可变形卷积的两种特例
###### 对于(c)加上offset，达到尺度变换的效果scale and (anisotropic) aspect ratio
###### 对于(d)加上offset，达到旋转变换的效果rotation
### $3\times 3$ deformable convolution
#### [illustration of 3x3 conv](https://i.imgur.com/90ECFxe.png)
#### 有一个额外的conv层来学习offset，共享input feature maps
#### 然后input feature maps和offset共同作为deformable conv层的输入，deformable conv层操作采样点发生偏移，再进行卷积
## Deformable ROI Pooling
### [[ROI pooling]] 是把不同大小的RoI($w\times h$)对应的feature map 统一到固定的大小($k\times k$)
### Deformable ROI pooling 则是先对RoI对应的每个bin按照RoI的长宽比例的倍数进行整体偏移,然后再pooling.
#### 同样偏移后的位置是小数，使用双线性差值来求
### 由于按照RoI长宽比例进行水平和竖直方向偏移，因此每一个bin的偏移量只需要一个参数来表示，具体可以用全连接来实现
### [deformable roi pooling](https://i.imgur.com/f7fd1hq.png)
### 如上图,ROI被分为$3\times 3$ 个bin,被输入到一个额外的fc层来学习offset,然后通过一个deformable RoI pooling层来操作使每个bin发生偏移