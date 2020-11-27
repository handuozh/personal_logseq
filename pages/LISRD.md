---
title: LISRD
public: true
---

## Meta Data
### [pautratOnlineInvarianceSelection2020]([[Online Invariance Selection for Local Feature Descriptors]])

### [[Pautrat, Rémi]], [[Larsson, Viktor]], [[Oswald, Martin R.]], & [[Pollefeys, Marc]] #authors

### [[2020]] #issued

### #title Online Invariance Selection for Local Feature Descriptors, 2020
### #topic #[[Keypoint]], [[Descriptor Extraction]], [[Descriptor-Matching]],  #NetVLAD

### #code https://github.com/rpautrat/LISRD

## Abstract
### Focus on descriptors only, use [[SIFT]] keypoints during training to propagate the gradient. 

### Limitation of feature descriptor is the trade-off of generalization and discriminative power: **more invariance means less informative descriptors**

### ^^Meta^^ descriptors encoding the regional variations
#### Similarity of them across images is used to select the right invariance.

## Method
### Select the most relevant variance for local feature descriptors

### 2 Steps:
#### Design a network to learn several dense descriptors, each with a type of invariance

#### Propose a strategy to determine the best invariance to use when matching.

### Architecture
#### ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FSLAM%2F_mcGOcnbPI.png?alt=media&token=4c1c25b2-889c-4baf-b40e-a99dae869e61)

#### Inspired by [[Superpoint]]

#### Computes

### 1. Disentangling invariance for local descriptors
#### Two factors: rotation and illumination

#### So there are four possible combinations of variance with respect to  illumination and rotation

#### the variant versions of descriptors are more discriminative since they are more specialized.

#### while the invariant ones are trading the discriminative power for better generalization capabilities.

#### Training losses
##### variants of [[margin triplet ranking loss]].

##### Dense descriptors are sampled on selected keypoints of the images, [[L2-norm]]alized to compute the losses.

### 2. Online Selection of the best invariance
#### During matching, explore how to pick the most relevant invariance

#### [[NetVLAD]] layer integration
##### Instead of naive approach to separately compute similarity of different local descriptors and pick the most similar ones

##### We need more context than single local descriptors, and need be consistent with neighboring descriptors

##### So extract regional descriptros from the local ones via [[NetVLAD]] to get a meta descriptor sharing the same kind of invariance as the subset of local descriptors.

##### Thus having similar meta descriptors means sharing the same level of variations.
###### Neighboring areas are tiles of $$c\times c$$ grids.

###### 4 meta descriptors per tile, then [[L2-norm]]alized.

#### We can rank the similarities of the 4 local descriptors (by scalar product)

#### Soft assignment (softmax)

#### ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FSLAM%2FvReSdGM4pv.png?alt=media&token=87a42d62-175f-4b0a-9413-b81d5b48d8a7)

#### ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FSLAM%2FNhRALW9YgQ.png?alt=media&token=b875b963-4ccc-44c2-b492-757f449bea76)

####