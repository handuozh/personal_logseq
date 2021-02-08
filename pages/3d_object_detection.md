---
title: 3D Object Detection
---

## Problem Statement of monocular 3D object detection [[problem description]]
:PROPERTIES:
:id: 6017cf83-cea5-4a5c-b118-55af96d8aa7e
:END:
### image $I$ with a set of $i=1, \cdots, N$ objects represented by 9 keypoints and other optional prior #RTM3D
#### Define keypoints as $\widehat{kp}_{ij}$
##### for $i\in{1,\cdots,9}$
#### dimension as $\widehat{D}_i$
#### orientation as $\hat{\theta}_i$
#### distance $\widehat{Z}_i$
### 3D bounding box $B_i$ can be defined by
#### rotation $R_i(\theta)$
#### position $\mathbf{T}_i=[T_i^x,T_i^y,T_i^z]^{\top}$
:PROPERTIES:
:id: 6017cf83-1500-4901-aa16-276935f2f6d0
:END:
#### dimensions $D_i=[h_i,w_i,l_i]^{\top}$
### **Goal**: estimate the 3D bounding box $B_i$
#### whose projections of center and 3D vertexes on the image plane best fit the corresponding 2D keypoints $\widehat{kp}_{ij}$
#### can be solved by minimizing the [[reprojection]] error of 3D keypoints and 2D keypoints
##### with nonlinear [[least squares]] problem
#####
$$R^*, T^*, D^* = \argmax\limits_{\{R,T,D\}} \sum\limits_{R_i,T_i,D_i}\big\| e_{cp}\left(R_i,T_i, D_i, \widehat{kp}_i \right)\big\|_{\Sigma_i}^2 \\+ \omega_d \big\| e_{d}\left(D_i, \widehat{D}_i \right)\big\|_{2}^2 + \omega_r \big\| e_{r}\left(R_i, \hat{\theta}_i \right)\big\|_{2}^2$$
##### where $e_{cp}(..), e_d(..),e_r(..)$ are measurement errorr of camera-point, dimension prior and orientation prior.
##### set $\Sigma$ covariance matrix of keypoints projection error
###### confidence extracted from the [[heatmap]] corresponding to the keypoints
#######
$$\Sigma_i=\rm{diag} \left(\rm{softmax} \left(V(\widehat{kp}_i^{1:8}),M(\widehat{kp}_i^9)\right)\right)$$
##
## Lidar based
:PROPERTIES:
:heading: true
:END:
### [[voxel R-CNN]]
## Monocular based
:PROPERTIES:
:heading: true
:END:
### [[disentangling transformation]]
### [[smoke]]
### Keypoint based monocular 3D object detection
#### predict 2D bbox center and offset from projected 3D bbox center.
#### [[RTM3D]]
#### [[RTM3D++]]
## Pseudo Lidar (depth prediction)
:PROPERTIES:
:heading: true
:END:
### [[3d detection with pseudo lidar]]
##
