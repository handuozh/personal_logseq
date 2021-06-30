---
title: ROI-10D
type: [[Conference paper]]
authors: [[Fabian Manhardt]], [[Wadim Kehl]], [[Adrien Gaidon]]
public: true
publication date: [[6/2019]]
citekey: manhardtROI10DMonocularLifting2019
tags: [[monocular]], [[3D Object Detection]], #zotero, #literature-notes, #reference
public: true
---

- Meta Data
  heading:: true
	- ROI-10D: Monocular Lifting of 2D Detection to 6D Pose and Metric Shape #toread
	- Zotero Metadata
		- * Proceedings Title: [[2019 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)]]
		- [https://ieeexplore.ieee.org/document/8953624/](https://ieeexplore.ieee.org/document/8953624/)
		- * DOI: [10.1109/CVPR.2019.00217](https://doi.org/10.1109/CVPR.2019.00217)
		- PDF Attachments
	- [Manhardt et al. - 2019 - ROI-10D Monocular Lifting of 2D Detection to 6D P.pdf](zotero://open-pdf/library/items/CU9Z8L7W)
		- [[abstract]]:
			- We present a deep learning method for **end-to-end** monocular 3D object detection and **metric shape retrieval**.
			- We propose a novel loss formulation by lifting 2D detection, orientation, and scale estimation into 3D space.
				- Instead of optimizing these quantities separately, the 3D instantiation allows to properly measure the metric misalignment of boxes.
				- We experimentally show that our ^^10D^^ lifting of sparse 2D Regions of Interests (RoIs) achieves great results both for
					- 6D pose
					- recovery of the textured metric geometry of instances.
				- This further enables **3D synthetic data augmentation** via
					- inpainting recovered meshes directly onto the 2D scenes.
			- We evaluate on KITTI3D against other strong monocular methods and demonstrate that our approach doubles the AP on the 3D pose metrics on the ofﬁcial test set, deﬁning the new state of the art.
		- zotero items: [Local library](zotero://select/items/1_HANSTTF3)
- Key ideas
  heading:: true
	- Concat depth map and coordinate map to RGB features + 2DOD + car shape reconstruction (6d latent space) for [[mono3D]]
	- 6 DoF pose + 3 DoF size + 1 DoF shape (but more like 6 dim) = 10D ROI.
		- Fully differentiable lifting mapping
			- $\mathbb{R}^4 \rightarrow \mathbb{R}^{8 \times 3}$ from 2D RoI $\mathcal{X}$ to a 3D box $\mathcal{B}:=\{B_1, \cdots, B_8\}$ of 8 ordered 3D points
	- Estimating absolute translation is^^ ill-posed due to scale and reprojection ambiguity^^.
		- In contrast, Global estimation of depth can be done due to geometric constraint as supervision.
	- Architecture
	  heading:: true
		- coord-map + depth map + feature map, RoI aligned to generate 3D detections. –> this is inferior to** lifting everything to 3D space**
	- When estimating the pose from monocular data only, little deviations in pixel space can induce big errors in 3D.
		- **Corner loss** is used to regularize overall learning.
	-
-