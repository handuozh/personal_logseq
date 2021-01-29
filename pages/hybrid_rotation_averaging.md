---
title: Hybrid Rotation Averaging
---

## Meta Data
:PROPERTIES:
:heading: true
:END:
### Hybrid Rotation Averaging: A Globally Guaranteed Fast and Robust Rotation Averaging Approach #toread

### Zotero Metadata

#### * Item Type: [[Article]]
#### * Authors: [[Yu Chen]], [[Ji Zhao]], [[Laurent Kneip]]
#### * Date: [[2021-01-22]]
#### [http://arxiv.org/abs/2101.09116](http://arxiv.org/abs/2101.09116)
#### * Cite key: chenHybridRotationAveraging2021
#### * Topics: [[Back End]]
#### * Tags: #Computer-Science---Computer-Vision-and-Pattern-Recognition #zotero #literature-notes #reference

#### PDF Attachments
##### [Chen et al_2021_Hybrid Rotation Averaging.pdf](zotero://open-pdf/library/items/MK7LBFZ4)
#### [[abstract]]:
:PROPERTIES:
:heading: true
:END:
##### We address rotation averaging and its application to real-world 3D reconstruction.
##### Optimization 的问题
###### Local optimisation based approaches are the defacto choice, though they only guarantee a **local optimum**.
###### Global optimizers ensure global optimality in low noise conditions, but they are inefficient and may easily deviate under the influence of **outliers or elevated noise levels**.
##### We push the envelope of global rotation averaging by formulating it as a [[semi-definite]] program that can be solved efficiently by applying the [[Burer-Monteiro]] method.
##### Both memory and time requirements are thereby largely reduced through a **low-rank factorisation**.
##### Combined with a fast view **graph filtering** as preprocessing, and a local optimiser as post-processing, the proposed hybrid approach is robust to outliers.
##### Compared against state-of-the-art globally optimal methods, our approach is 1 ~ 2 orders of magnitude faster while maintaining the same or better accuracy. We apply the proposed hybrid rotation averaging approach to incremental [[SFM]] by adding the resulting global rotations as [[regularizer]]s to bundle adjustment. Overall, we demonstrate high practicality of the proposed method as bad camera poses are effectively corrected and drift is reduced.
#### zotero items: [Local library](zotero://select/items/1_QIZK7ZZL)
## Motivation
### Local
