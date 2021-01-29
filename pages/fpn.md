---
title: FPN
alias: Feature Pyramid Network
tags: CNN
---

## #reference  Feature Pyramid Networks for [[object detection]]
## Detect multi-scale 2D box in different pyramid layers
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
####
