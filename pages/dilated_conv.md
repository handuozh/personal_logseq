---
title: dilated conv
---

## 空洞卷积
## FCN先像传统的CNN那样对图像做卷积再pooling，降低图像尺寸的同时增大感受野
### 由于图像分割预测是pixel-wise的输出，所以要将pooling后较小的图像尺寸upsampling到原始的图像尺寸进行预测，之前的pooling操作使得每个pixel预测都能看到较大感受野信息
### ![2020_12_28_standard.png](https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_12_28_standard.png?Expires=4762720645&Signature=hkyk35~Ih3auqiAfwN42m~ThIHgcui83S4EaPupQMgjj24sbEIBoZkPLt4L~K804dh3f~UAiS-b~r2LYvJ1lUp3DIqywq67BPdiKEY68kMFFyTEC1m1FY6K92SjDnkUMGcrvXYIOUnGFQRWAUOIS4KqWzvVz9-bShKgFcOMJAlnvyh6RSx3TGmf1iUk2W2oFdqCIg8R3mW4NPW~AzRvraHwctY-uRCaCJcDVwPTbJ7Ik71VE66ffWGVbxRrq0vXdDj7OMCZxfoEdsjujza4DwB~wa5oTWeC3gBWSLTSA4y75GHiVPWhref7QdctxWnmAErDDhsBPgRhA9DXFyOD9gA__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA)
### 因此图像分割FCN中有两个关键
#### pooling减小图像尺寸增大感受野
#### upsampling扩大图像尺寸
### 主要问题
:PROPERTIES:
:heading: true
:END:
#### Up-sampling / pooling layer (e.g. bilinear interpolation) is deterministic. (参数不可学习)
#### 内部数据结构丢失；空间层级化信息丢失
#### 小物体信息无法重建 (假设有四个pooling layer 则 任何小于 2^4 = 16 pixel 的物体信息将理论上无法重建.)
####
### 在先减小再增大尺寸的过程中，肯定有一些信息损失掉了，那么能不能设计一种新的操作，不通过pooling也能有较大的感受野看到更多的信息呢？答案就是[[dilated conv]]
### ![2020_12_28_dilated.png](https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_12_28_dilated.png?Expires=4762720632&Signature=F4-BqYwHLu~PFFjbZRAa3~jdqBx~VM1focBB~2y0UNA4qk4WqmPCDHlqKOKgoj9OZktRMQUYZEo2BLR18F7On57ePLaCMYBh5Fk7UtvNiNVn-FJMOAyq2E-ptQ9pPKx9vbWW9qRBCvD7rSdtrFibeqaLIFVIqDiqNNgfl1IL2mlO2EWftiETRbEZ7yuOPZcMGMPZJrXzd1pRxJPp-4fhbxZ~5Vxan2ruaa9pavWjd0Si7N82RdklHFpElhGidlHHLuvFnOELcrxTYVrKow84J0bpBfiPARtoUTkn2vAGsXsWDkBh7tE2-Qwzf4T~GqhJvkRrzEO6~SQ-yjv-rElZCQ__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA)
###