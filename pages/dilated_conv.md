---
title: dilated conv
---

## 空洞卷积
## FCN先像传统的CNN那样对图像做卷积再pooling，降低图像尺寸的同时增大感受野
### 由于图像分割预测是pixel-wise的输出，所以要将pooling后较小的图像尺寸upsampling到原始的图像尺寸进行预测，之前的pooling操作使得每个pixel预测都能看到较大感受野信息
### 因此图像分割FCN中有两个关键
#### pooling减小图像尺寸增大感受野
####
