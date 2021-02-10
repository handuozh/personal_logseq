---
title: disentangling transformation
tags: [[literature-notes]], paper, monocular, zotero, [[3D Object Detection]], mono3D, reference
public: true
type: [[Conference paper]]
citekey: simonelliDisentanglingMonocular3D2019
publication date: [[10/2019]]
authors: [[Andrea Simonelli]], [[Samuel Rota Bulo]], [[Lorenzo Porzi]], [[Manuel Lopez-Antequera]], [[Peter Kontschieder]]
---

## Meta Data
### Disentangling Monocular 3D Object Detection #toread
### Zotero Metadata

#### Proceedings Title: [[2019 IEEE/CVF International Conference on Computer Vision (ICCV)]]
#### [https://ieeexplore.ieee.org/document/9010618/](https://ieeexplore.ieee.org/document/9010618/)
#### * DOI: [10.1109/ICCV.2019.00208](https://doi.org/10.1109/ICCV.2019.00208) 

#### PDF Attachments
	- [Simonelli et al. - 2019 - Disentangling Monocular 3D Object Detection.pdf](zotero://open-pdf/library/items/YJ67ZYLI)

#### [[abstract]]:
##### In this paper we propose an approach for monocular 3D object detection from a single RGB image, which leverages a novel disentangling transformation for 2D and 3D detection losses and a novel, [[Self-supervised]] conﬁdence **score** for 3D bounding boxes.
###### Our proposed loss disentanglement has the twofold advantage of simplifying the training dynamics in the presence of losses with complex interactions of parameters and sidestepping the issue of balancing independent regression terms.
###### 2D检测->3D检测->loss解耦
##### Our solution overcomes these issues by isolating the contribution made by groups of parameters to a given loss, without changing its nature.
###### 解耦的 regression loss，用来替代之前同时回归 center、size、rotation 带来的由于各个 opponent 的 loss 大小不同导致的训练问题
###### 将回归的部分分成 k 个 group，每个 group 只有自身的参数需要学习，其他的部分使用 gt 代替
###### 从而实现每个分支只回归某一个 component，使得训练更加稳定
##### We further apply loss disentanglement to another novel, signed [[intersection over union]] criterion-driven loss for improving 2D detection results.
###### 将没有 overlap 的 bboxes 的 loss 也考虑进来
##### Besides our methodological innovations, we critically review the AP metric used in KITTI3D, which emerged as the most important dataset for comparing 3D detection results. We identify and resolve a ﬂaw in the 11-point interpolated AP metric, affecting all previously published detection results and particularly biases the results of monocular 3D detection. We provide extensive experimental evaluations and ablation studies and set a new state-of-the-art on the KITTI3D Car class.
#### zotero items: [Local library](zotero://select/items/1_2D9DZC3A)
##
##
##
##
##
## Disentangling Transformation
:PROPERTIES:
:heading: true
:END:
### $L: \mathcal{Y} \times \mathcal{Y} \rightarrow \mathbb{R}_{+}$ loss function
#### defined on a space $\mathcal{Y}$ (space of 3D bbox)
#### $L(y,\hat{y})=0$ if $\hat{y}=y$
### 2D bbox case
#### 2 groups: dim and center
### 3D bbox case
#### 4 groups: depth, projected center, rotation and dim
### $\psi(\theta_j,\hat{\theta}_{-j})$ the mapping of a parametrization that takes the $j$th group from $\theta$ and the rest of the params from $\theta^{\prime}$.
####
$$L_{dis}(y,\hat{y})=\sum\limits_{j=1}^{k} L\left(\psi(\theta_j,\hat{\theta}_{-j}), \hat{y}\right)$$
##### where $\theta=\psi^{-1}(y)$ and $\hat{\theta}=\psi^{-1}(\hat{y})$
##### 把要回归的loss分成k组，每一组计算时使用output$_j$, gt$_{-j}$，计算出对应的smooth [[l1 loss]]
##### 解耦不同语义的各部分的相互影响，使得收敛过程更加平滑，训练更加稳定
