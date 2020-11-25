---
title: D2-Net
---

## Meta Data
### [dusmanuD2NetTrainableCNN2019]([[D2-Net: A Trainable CNN for Joint Description and Detection of Local Features]])

### [[Dusmanu, Mihai]], [[Rocco, Ignacio]], [[Pajdla, Tomas]], [[Pollefeys, Marc]], [[Sivic, Josef]], [[Torii, Akihiko]], & [[Sattler, Torsten]] #authors

### [[2019]] #issued

### #title [[D2-Net: A Trainable CNN for Joint Description and Detection of Local Features]]

### #publisher [[IEEE]] [[CVPR]]

### #[[topic]]: #Keypoint #[[Keypoint Extraction]]  #[[Descriptor Matching]]  #Detection-and-describe

## [[Detect-then-describe]]:: Classical interest point detection and description method where separated handcrafted methods are used to first identify repeatable keypoints and then represent them with a local descriptor. #definition

## [[Detection-and-describe]]  paradigm
### Compare two paradigms
![https://remnote-user-data.s3.amazonaws.com/9fLcaaEHF93T7XMf6h9zTACHMAqDIGaPhZ4_giEAAXE9_ddPlTjg--DrL21MB6i9jvK3bJ9hP0nJ06MwOsoINiQMsnhGzy16uUtaLDVJFHfYYNE5xp-6C9oqOYbnMwdH](https://remnote-user-data.s3.amazonaws.com/9fLcaaEHF93T7XMf6h9zTACHMAqDIGaPhZ4_giEAAXE9_ddPlTjg--DrL21MB6i9jvK3bJ9hP0nJ06MwOsoINiQMsnhGzy16uUtaLDVJFHfYYNE5xp-6C9oqOYbnMwdH)  

## In the paper both detector and descriptor share the underlying representation, as shown below: 
![https://remnote-user-data.s3.amazonaws.com/seSpWmYsDTI-0ZXFmMri4soNBrZ6fyVocu2YGf9kqyjFAKHKCrg56WBNzky9ss8f-X680dHJSnyamzf_lK5EzkJBylMqw3rQl9fzs-GBNShbC8RMscMu6N3i-XwkZan5](https://remnote-user-data.s3.amazonaws.com/seSpWmYsDTI-0ZXFmMri4soNBrZ6fyVocu2YGf9kqyjFAKHKCrg56WBNzky9ss8f-X680dHJSnyamzf_lK5EzkJBylMqw3rQl9fzs-GBNShbC8RMscMu6N3i-XwkZan5)  

## 1. Feature Description
### Interpretatioin of feature 3D tensor $$F$$ is a dense set $$\mathbf{d}$$:
#### (1)    $$ \mathbf{d}_{ij} = F_{ij}, \mathbf{d} \in {\mathbb{R}^n}$$
where $$ n $$ is channel size, $$ i=1,\cdots, h $$, $$ j=1,\cdots,w $$  

### Compare using [[L2-norm]]

## 2. Feature Detection
### Collection of 2D responses $$ D $$ from 3D tensor $$ F $$ :
#### (2)   $$D^k=F_{: :k},   \; D^k \in{\mathbb{R}^{h\times w}}$$

#### where $$ n $$ is channel size, $$ k==1,\cdots,n $$  

### 2.1 Hard Feature Detection
#### For a point $$ (i,j) $$ to be detected, we need:$$$$
##### (3)      $$ (i,j) $$ is a detection $$ \Longleftrightarrow D_{ij}^k $$ is a local max in $$ D^k $$
 with $$ k=\argmax\limits_t D_{ij}^t $$  

### 2.2 Soft Feature Detection
#### Amenable for back propagation.

#### Define a soft local-max score
##### (4)            $$ \alpha_{ij}^k=\exp(D_{ij}^k)/\sum_{(i^{\prime},j^{\prime})\in\mathcal{N}(i,j)} \exp(D_{i'j'}^k) $$
where $$ \mathcal{N}(i,j) $$ is the set of 9 neighbours of pixel $$ (i,j) $$.

#### Define a soft channel selection to compute a [Ratio-to-max](Ratio-to-max.md) per descriptor that emulates channel-wise non-maximum suppression:
##### (5)            $$ \beta_{ij}^k = D_{ij}^k/\max\limits_t D_{ij}^t $$  

#### Together we maximize the product of both scores across all feature maps $$ k $$ to obtain single score map:
##### (6)            $$ \gamma_{ij}=\max\limits_k (\alpha_{ij}^k \beta_{ij}^k $$)

#### Image-level normalization:
##### (7)             $$ s_{ij}=\gamma_{ij} / \sum\limits_{(i',j')} \gamma_{i'j'} $$

### 2.3 Multiscale Detection

## 3. Jointly Optimizing detection and description