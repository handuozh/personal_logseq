---
title: Learning to Segment Rigid Motions from Two Frames
tags: Article, [[motion segmentation]], #zotero, #literature-notes, #reference
type: Article
publication date: [[2021-01-10]]
citekey: yangLearningSegmentRigid2021
authors: [[Gengshan Yang]], [[Deva Ramanan]]
---
## Learning to Segment Rigid Motions from Two Frames #reading
### Zotero Metadata

#### * Date: 
#### [http://arxiv.org/abs/2101.03694](http://arxiv.org/abs/2101.03694)
####  
#### PDF Attachments
	- [Yang_Ramanan_2021_Learning to Segment Rigid Motions from Two Frames.pdf](zotero://open-pdf/library/items/JZPT2ZF7)

#### [[abstract]]:
Appearance-based detectors achieve remarkable performance on common scenes, but tend to fail for scenarios lack of training data. Geometric motion segmentation algorithms, however, generalize to novel scenes, but have yet to achieve comparable performance to appearance-based ones, due to noisy motion estimations and degenerate motion configurations. To combine the best of both worlds, we propose a modular network, whose architecture is motivated by a geometric analysis of what independent object motions can be recovered from an egomotion field. It takes two consecutive frames as input and predicts segmentation masks for the background and multiple rigidly moving objects, which are then parameterized by 3D rigid transformations. Our method achieves state-of-the-art performance for rigid motion segmentation on KITTI and Sintel. The inferred rigid motions lead to a significant improvement for depth and scene flow estimation. At the time of submission, our method ranked 1st on KITTI scene flow leaderboard, out-performing the best published method (scene flow error: 4.89% vs 6.31%).

#### zotero items: [Local library](zotero://select/items/1_TZCQLLB2)
## Abstract
:PROPERTIES:
:heading: true
:END:
### Segment rigid bodies from two frames
### 3D motion parameterization
### model deformable objects as rigid body over short time
#### Or decouple into rigidly-moving parts
### 传统 rigid [[motion segmentation]] challenges
:PROPERTIES:
:id: 602cf422-e94b-4cb7-801e-7f18a44e20ed
:END:
#### 1. [[epipolar geometry]] constraints fail when camera motion is close to zero
#### 2. Points moving along [[epipolar line]] cannot be distinguished from rigid background
#### 3. geometric criteria are often not robust enough to noises
### analyze **ambiguities** in 3D motion segmentation
#### upgrade 2D motion observation to 3D with
##### [[optical expansion]]
##### [[monocular depth]] cues
#### Deal with noisy motion correspondences and degenerate scene motion
##### design a CNN architecture from a given motion field
## Two-Frame 3D Motion Segmentation
:PROPERTIES:
:heading: true
:END:
### Given motion correspondences $(\mathbf{p}_0, \mathbf{p}_1) \in{\mathbb{R}^4}$
#### Observed by monocular camera with known camera intrinsics $(\mathbf{K}_0,\mathbf{K}_1)$
#### Target is to detect points whose 3D motion cannot be described by camera motion $\mathbf{R}_C \in {\mathbf{SO}(3)}, \mathbf{T}_C \in {\mathbb{R}^3}$
##### such that ^^(1)^^ $(\mathbf{R_C P_1}+\mathbf{T}_C)-\mathbf{P}_0 \neq 0$
##### $\mathbf{P}_0=Z_0 \mathbf{K}_0^{-1}\tilde{\mathbf{p}_0}$ and $\mathbf{P}_1=Z_1 \mathbf{K}_1^{-1}\tilde{\mathbf{p}_1}$ are corresponding 3D points observed in camera frame
:PROPERTIES:
:id: 5ffeb0b3-fc27-46ef-8016-74cebe53cc76
:END:
###### depth $(Z_0, Z_1)$
###### homogeneous coordinates $(\tilde{\mathbf{p}_0},\tilde{\mathbf{p}_1})$
##### 其实就是不符合 [[epipolar geometry]] 约束的运动 ->动态点(物体)
#### 重写(1)
##### ^^(2)^^ $\mathbf{T}_{sf}=\mathbf{K}_0^{-1} \left(Z_1 \mathbf{H}_R \tilde{\mathbf{p}}_0 \right) \neq - \mathbf{T}_C$
##### "rectified" [[3D scene flow]] $\neq$ negative camera translation
##### where $\mathbf{T}_{sf}=\mathbf{R_c P}_1 - \mathbf{P}_0$ is the "rectified" 3D scene flow,
###### with motion induced by camera rotation removed through "recitifying" the second frame coordinate to have the same orientation as 1st
##### $\mathbf{H}_R=\mathbf{K}_0 \mathbf{R}_C \mathbf{K}_1^{-1}$  is the rotational homography that "rectifies" the second image plane into the same orientation as 1st frame
##### There are two crucial [[degrees of freedom]] undetermined
###### depth $Z_0$ and $Z_1$
### Coplanar [[motion degeneracy]]
:PROPERTIES:
:heading: true
:END:
#### 传统的解决3D scene flow的方法
##### segment points whose 2D moition is inconsistent with camera motion
###### measured by [[Sampson distance]] to the [[epipolar line]]
###### or measured by [[plane plus parallax]] representations that factor out camera rotation
##### 但是2D motion用来做3D segmentation会有问题
##### ![image.png](/assets/pages_learning_to_segment_rigid_motions_from_two_frames_1610532171258_0.png)
##### ![image.png](/assets/pages_learning_to_segment_rigid_motions_from_two_frames_1610532212322_0.png)
###### 相对于camera来说,背景在运动 $T_{bg}=-T_c$
###### $T_{sf}=T_{bg}+T_o$ 就是3D scene flow
###### (1) 如果$T_o=0$ 物体没有动
####### projected 2D motion lies on the [[epipolar line]]
###### (2) 如果2D的motion 脱离了epipolar line ($|\alpha|>\alpha_0$), 那么$\mathbf{P}$肯定是在运动
####### 参见 [[Sampson distance]]
###### (3)
#### 为了解决这个问题引入 [[optical expansion]]
#####