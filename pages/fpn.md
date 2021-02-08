---
title: FPN
alias: Feature Pyramid Network
tags: CNN
public: true
---

## #reference  Feature Pyramid Networks for [[object detection]]
## Detect multi-scale 2D box in different pyramid layers
### 每个layer都包含不同内核大小的卷积滤波器,以扩大 [[receptive field]]并集成更多有用的信息
## pyramid of [[feature map]]s
### ![image.png](/assets/pages_fpn_1611838411462_0.png){:height 228, :width 273}
### feature maps closer to the image layer (low level structures) are not effective for accurate object detection
## FPN does not use feature extractor of detectors like [[Faster R-CNN]]
### generates multiple [[feature map]] layers (multi-scale feature maps)
### ![image.png](/assets/pages_fpn_1611838575183_0.png)
### 有两个pathway
#### bottom-up -> feature extraction
##### -> decrease spatial resolution -> higher level -> higher semantic value
#### top-down -> higher resolution reconstructed layers -> better localization
### Lateral connections between reconstructed layers and corresponding feature maps
#### also act as [[skip connection]]s to make training easier (like [[ResNet]])
## 一片paper[[DyFPN]]指出并非所有对象都需要复杂的计算模块
##