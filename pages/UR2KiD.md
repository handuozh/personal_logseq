---
title: UR2KiD
public: true
---

## Meta Data
:PROPERTIES:
:heading: true
:END:
### #title UR2KiD: Unifying Retrieval, Keypoint Detection, and Keypoint Description without Local Correspondence Supervision, 2020
### #topic #Keypoint #Detection-and-describe #[[Keypoint Extraction]]  #[[Descriptor-Matching]]  #[[Image Retrieval]] #[[Self-distillation]]

## Abstract
:PROPERTIES:
:heading: true
:END:
### Combine keypoint detection, description, and image retrival jointly in a single unified framework.

### Leverage diverse info from ResNet architecture, extract keypoints and descriptors that encode local info using [[Local Activation Norms]], [[Channel Grouping]], [[Channel Droping]], and [[Self-distillation]].

### Global info for [[Image Retrieval]] is encoded on ^^pooling of the local responses^^. 

### Method does not rely on pointwise/ pixelwise correspondences.

### No supervision needed. Does not need depth-maps from an SfM model nor manually created synthetic affine transformations.

## **Structure**: generate local and global description in a network pipeline $$ P_{joint} $$.
### Given training supervision: [[Anchor Sample]] $$ a $$, [[Positive Sample]] $$ p $$ and [[Negative Sample]] $n_k$ ^^without^^ pixelwise or pointwise matching correspondences.  ^^Image pairs are from SfM model, no GT needed.^^
### $$ d_g, \{k_i, d_i\}=P_{joint}(I)$$

### Overview (with backbone pretrained CNN [[ResNet 101]])
#### [[https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_11_27_train.png?Expires=4760063782&Signature=IzkKGRRLA5QDRuwFrAcdStCrxIcFO4CaSu9K1~uzQy6M1CCxBxhFc1zFlR6GNGsVKTi6LvILChQMX-ddnG49-k2ExeYerSRxvGbyWjL5a6JKAfaC66QNp2crlMk2jLJHn-YbSREp4VjakUdEHl3nbBvB5KiP2FWEhBiVEa5neg2SNkIUhxMRu1i-1bBv65VUBHf4D4e0HUXI63euJUHVcl3yGH-dfvbREUvawLRnetIkD5UrPN5E8YaMkRGlU8W0nvpaS~cJeeztqPa~CoJUyUDdyBM~H8YXUFA6yc7KuG~Ua2aU3kBNlPzSPBI35XacMXjVvgqyUMF9x1u3EdFp0w__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA][2020_11_27_train.png]]
#### [[https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_11_27_test.png?Expires=4760063770&Signature=HhOEyWWmQ4wDycnGbbTPuYLVHEcraUr90PAnw96luo1Q8MIXYpf2KMSRM7gL1-1nNIRMxM0yidNvfDr~DZ-nAiUk1Y8MhGAhKpQe3BB-1d4o41Qf6goRTo9KEwvo8Q02N3yuQeoD~fA5aSeFORy6gQSuylj20kLEX2oWjL~MTqQqP9FgBk1No2j3mr2lqtdV64Lc5N7dWfu1~-l6XT2xmDwdYde9ujRr-kKmSL3R80UjS19nB-74dl6dDKpl4bVwMPyIcbMwX1RoKRGI1Qv-gOHnmZsm70ihasAWZB8QMF7IK2ONlFzQQJNSglrZwbRFvJvjUO0daamalctshD3rqA__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA][2020_11_27_test.png]]
#### Multi-level feature extraction for local descriptors from ResNet blocks and [FPN](FPN.md).

#### Final global descriptor is the concatenation of multiple different [GeM](GeM.md) pooling results.

## 1. Local [[Keypoint Extraction]]
### Based on [[D2-Net]] the idea of [[Detection-and-describe]].

### Keypoint confidence of $$ i $$-th keypoint coordinate out of feature map $$ F $$ is computed by thresholding the maximum response $$ F_{(x_i,y_i)}^{c^*} $$ ^^across channel dimension^^ $$ c^*=\argmax _c $$$$F_{(x_i,y_i)}^{c^*} $$$$ $$along with local [[NMS]] and edge confidence thresholding of [[Harris Corner]] detector.

### Based on the idea of ^^independent matching for each channel^^, aggregate the concept from different channels as [[GC-DAD]] for both keypoint detection and description.

### **GC-DAD**:
#### Group-Concept Detect-and-Describe, [[Channel Grouping]] method.  

#### Unlike [[D2-Net]] taking maximum value across the feature map channel, this paper depends on [[L2 Response]] as the importance of keypoint.

#### Divide feature map into different groups as independent concepts $$ \{F\}^g $$ with $$ g $$ as the group index.

#### If a feature map $$ F $$ contains $$ K $$ channels and we want to uniformly divide into $$ G $$ groups, then $$ F^{g=1} $$ is corresponding to channel $$ 1\sim \lfloor \frac{K}{G}\rfloor   $$ and $$ F^g $$ corresponds to channel$$$$$$ \left((g-1)*\lfloor \frac{K}{G}\rfloor+1\right)\sim\left(\lfloor \frac{K}{G}\rfloor*g\right)$$.$$$$  

#### $$ i $$-th keypoint location out of the [[L2 Response]] map of feature map $$ F^g $$ can be computed.  

#### Combine the keypoint sets from every group by setting a [[L2 Response]] threshold on the [[L2-norm]] of each group $$ F_{(x_i,y_i)}^g $$ along with Harris edge threshold and 2-nd order displacement.

#### Challenges::
##### Final descriptor is memory costly since GC-DAD only detects keypoints and the channel dimension is preserved, out of the given freature map.

##### Manually selected groups is not the optimal choice since there is no clue how to aggregate them.

##### Solution: [[Concept Dropout Dimension Reduction]]

### [[Concept Dropout Dimension Reduction]] 
#### Randomly drop the feature channels in the training phase.

#### Conceptually selecting different groups without manually selection, brings the diverse concepts into matching and avoid overfitting.

#### $$ \hat{F}=\text{Conv}\left(\text{Drop}_{2D}(F)\right) $$  with feature map $$ F $$ the high dimemsion input and $$ \hat{F} $$ the low dim results.

## 2. [[Descriptor-Matching]]
### Matching loss for affinity matrix

### For training the discriminative local descriptor ([[reliability]], given different input image $$ a, p $$, and corresponding feature maps $$ \hat{F}_a, \hat{F}_p $$ along with the matching affinity matrix$$ M $$ can be computed by [[Inner Product]].  

### $$ M=\hat{F}_a \cdot \hat{F}_p^{\top} $$ 

## 3. Cross Dimension Distillation

## During feature mapping, low dim descriptor $$ \hat{F} $$ cannot capture whole info from $$ F $$ by only using matching loss.

## **Distillation**: Transfer info from teacher model to student model, as in [[HF-Net]], with the extra dataset to train.
## Here distill indirectly:

## $$ L_{Dis}=\left( M_{high} \cdot M_{high}^{(det)}-\hat{M}_{low}\right)^2 $$
### The distillation happens on the matching affinity matrix $$ M_{high} $$ and $$ \hat{M}_{low} $$ with corresponding soft detection score $$ M_{high}^{(det)} $$ for the high dimentional feature $$ F $$ as described in [[D2-Net]].