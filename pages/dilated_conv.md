---
title: dilated conv
---

## 空洞卷积
## FCN先像传统的CNN那样对图像做卷积再pooling，降低图像尺寸的同时增大感受野
### 由于图像分割预测是pixel-wise的输出，所以要将pooling后较小的图像尺寸upsampling到原始的图像尺寸进行预测
#### upsampling一般采用[[deconv]]反卷积操作，deconv可参见CVPR2016 Shallow and Deep Convolutional Networks for Saliency Prediction (https://www.zhihu.com/question/43609045/answer/132235276)
### 之前的pooling操作使得每个pixel预测都能看到较大感受野信息
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
### 在先减小再增大尺寸的过程中，肯定有一些信息损失掉了那么能不能设计一种新的操作，不通过pooling也能有较大的感受野看到更多的信息呢？答案就是[[dilated conv]]
#### ![2020_12_28_dilated.png](https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_12_28_dilated.png?Expires=4762720632&Signature=F4-BqYwHLu~PFFjbZRAa3~jdqBx~VM1focBB~2y0UNA4qk4WqmPCDHlqKOKgoj9OZktRMQUYZEo2BLR18F7On57ePLaCMYBh5Fk7UtvNiNVn-FJMOAyq2E-ptQ9pPKx9vbWW9qRBCvD7rSdtrFibeqaLIFVIqDiqNNgfl1IL2mlO2EWftiETRbEZ7yuOPZcMGMPZJrXzd1pRxJPp-4fhbxZ~5Vxan2ruaa9pavWjd0Si7N82RdklHFpElhGidlHHLuvFnOELcrxTYVrKow84J0bpBfiPARtoUTkn2vAGsXsWDkBh7tE2-Qwzf4T~GqhJvkRrzEO6~SQ-yjv-rElZCQ__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA)
### ![2020_12_28_compare.png](https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_12_28_compare.png?Expires=4762720900&Signature=M4xjtTMZ7NxBI6vyb7G80VOTwRoKe3wqLbizMzLHQfvbz6W6O8Gtc4rbHRXoKDZxhzk5vN54E25~N~mfICtkMGNQoED8VJIbdYTj3s6ZP2NMbth5epQgfB4-lEuMmYJ3kCFAApgvioyW4WzqWJGGqOl8suW5dbVpRT8pkmRMdz8aIP9pGrriSJ8WndwHY4Hs9H5e3z40skWGwBHQDObxsknzuycqLlOlFkqOfbkU0yRcvNjX~FQJJEKugSePgzyo~22fwEpCE7pwAuIbK6evUVGVeaJpwiWjh4kOK5A1SWxtH6S-15HbmLaGUwWl87RJQMFrpcvZPrCUegjYndkHvg__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA)
## 缺点
:PROPERTIES:
:heading: true
:END:
### 1. The Gridding Effect
#### 假设我们仅仅多次叠加 dilation rate 2 的 3 x 3 kernel 的话，则会出现这个问题
#### ![2020_12_28_gridding.jpg](https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_12_28_gridding.jpg?Expires=4762721737&Signature=g5RywuH5voI1c8d~0KQ9pgK1blP1AYo2kHFwr2I-DzCx0Y-oRFkDOIv~v2XcUtEsctTOsKtH2exPHG8THArIG6MRVJb8YJcg~e~WR-QTwnCTNCRyhe4w4d4mKXEaby~~3XzH37T3kUlyxlBSloCLEn9r6PtqeIuO~43h6A63U6Ul6ufdar97wNewnQ-ccw4KGNosPsTzeEloKVj9aTNCkP7XlUrBvgxbQDYxHM~izIMcidkZp-~T~wyLaz-F7OV23BT2J50lfqWKFMVIwZMawijW~8j-PmWJFWk600omTLNZwSQVLDAPU1sW~t5ZxsqCMb2TkSPhSVdNrBdf06DSlw__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA)
#### kernel 并不连续，也就是并不是所有的 pixel 都用来计算了，因此这里将信息看做 checker-board 的方式会损失信息的连续性.
#### 这对 pixel-level dense prediction 的任务来说是致命的
### 2. Long-ranged information might be not relevant
####
