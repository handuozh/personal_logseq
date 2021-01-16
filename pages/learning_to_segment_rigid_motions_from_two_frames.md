---
title: Learning to Segment Rigid Motions from Two Frames
---

## Topic

## Abstract
:PROPERTIES:
:heading: true
:END:
### Segment rigid bodies from two frames
### 3D motion parameterization
### model deformable objects as rigid body over short time
#### Or decouple into rigidly-moving parts
### 传统 rigid [[motion segmentation]] challenges
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
