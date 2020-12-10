---
title: VDO_SLAM
---

## 1. Notion
:PROPERTIES:
:heading: true
:background_color: rgb(38, 76, 155)
:END:
### Coordinate Frames
:PROPERTIES:
:heading: true
:END:
#### Let ${}^0 \mathbf{X}_k,{}^0 \mathbf{L}_k\in{\mathbb{SE}(3)}$ be the robot and object 3D pose respectively, at time $k\in{\mathcal{T}}$ in global reference frame $0$.
### Points in 3D space
:PROPERTIES:
:heading: true
:END:
#### Let ${}^0 \mathbf{m}_k^i$ be the homogeneous coordinates of the $i^{th}$ 3D point at time $k$ with ${}^0\mathbf{m}^i=\left[m_x^i,m_y^i,m_z^i,1 \right]^{\top} \in{\text{IE}^3}$ and $i\in {\mathcal{M}}$ the set of points.
#### So a point in robot frame as ${}^{X_k}\mathbf{m}_k^i={}^0\mathbf{X}_k^{-1}\cdot  {}^0 \mathbf{m}_k^i$.
#### Define $\mathbf{I}_k$ the reference frame associated with image captured by the camera at time $k$ chosen at the top left corner of the image, and let ${}^{I_k}\mathbf{p}_k^i=\left[u_i,v_i,1 \right]\in{\text{IE}^2}$ be the pixel location on frame $\mathbf{I}_k$ corresponding to the homogeneous 3D point ${}^{\mathbf{X}_k}\mathbf{m}_k^i$, which is obtained via projection function $\pi(\cdot)$
#### ^^(1)^^  $${}^{I_k}\mathbf{p}_k^i=\pi\left({}^{X_k}\mathbf{m}_k^i\right)=\mathbf{K}{}^{\mathbf{X}_k}\mathbf{m}_k^i$$
### Optical flow ${}^{I_k}\mathbf{\phi}_i$ indicates ^^motion^^ of pixel ${}^{I_{k-1}}\mathbf{p}_{k-1}^i$ from image frame $I_{k-1}$ to $I_k$
:PROPERTIES:
:heading: true
:END:
#### ^^(2)^^  $${}^{I_k}\mathbf{\phi}_i={}^{I_{k}}\tilde{\mathbf{p}}_{k}^i-{}^{I_{k-1}}\mathbf{p}_{k-1}^i$$
#### ${}^{I_{k}}\tilde{\mathbf{p}}$ is the correspondence of ${}^{I_{k-1}}\mathbf{p}_{k-1}^i$ in $I_k$.
### Object and 3D Point Motions
#### The object motion is homogeneous transformation ${}^{L_{k-1}}_{k-1}\mathbf{H}_k\in{\mathbb{SE}(3)}$ where
#### ^^(3)^^  $${}^{L_{k-1}}_{k-1}\mathbf{H}_k={}^0 \mathbf{L}_{k-1}^{-1} \cdot {}^0 \mathbf{L}_k$$
## 2. Camera Pose ${}^0 \mathbf{X}_k$
:PROPERTIES:
:heading: true
:background_color: rgb(73, 118, 123)
:END:
### Given static 3D points $\{{}^0\mathbf{m}_{k-1}^i | i\in{\mathcal{M}}, k\in{\mathcal{T}}\}$ observed at time $k-1$ in global reference frame.
### 2D correspondences $\{{}^{I_k}\tilde{\mathbf{P}}_k^i | i\in{\mathcal{M}},k\in{\mathcal{T}}\}$ in image $\mathbf{I}_k$.
### re-projection error of camera pose ${}^0 \mathbf{X}_k$ is:
#### ^^(7)^^  $$\mathbf{e}_i({}^0 \mathbf{X}_k)=\{{}^{I_k}\tilde{\mathbf{P}}_k^i-\pi\left({}^0\mathbf{X}_k^{-1} \cdot {}^0\mathbf{m}_{k-1}^i\right)$$
##### Parameterise the SE(3) camera pose by elements of [[Lie-algebra]] $\mathbf{x}_k \in{se(3)}$
###### ^^(8)^^  $${}^0 \mathbf{X}_k=\exp({}^0\mathbf{x}_k)$$
##### Define ${}^0\mathbf{x}_k^{\vee}\in{IR^6}$ mapping from se(3) to $IR^6$
### [[least squares]] **cost** is:
#### ^^(9)^^  $${}^0\mathbf{x}_k^{*\vee}=\argmin\limits_{{}^0\mathbf{x}_k^{\vee}}{\sum\limits_{i}^{n_b}\rho_h\left( \mathbf{e}_i^{\top}({}^0\mathbf{x}_k)\Sigma_p^{-1}\mathbf{e}_i({}^0\mathbf{x}_k)\right)}$$
#### $n_b$ visible 3D-2D static background point correspondences.
## 3. Object motion ${}^0_{k-1}\mathbf{H}_k$
:PROPERTIES:
:background_color: rgb(73, 125, 70)
:heading: true
:END:
### Reprojection error of object 3D point and corresponding 2D point in image $\mathbf{I}_k$:
####
$$\mathbf{e}_i({}^0_{k-1} \mathbf{H}_k)\begin{aligned}=& {}^{I_k}\tilde{\mathbf{P}}_k^i-\pi\left({}^0\mathbf{X}_k^{-1} \cdot {}^0_{k-1} \mathbf{H}_k \cdot {}^0 \mathbf{m}_{k-1}^i\right) \\
& ={}^{I_k}\tilde{\mathbf{P}}_k^i-\pi\left( {}^0_{k-1} \mathbf{G}_k \cdot {}^0 \mathbf{m}_{k-1}^i\right)\end{aligned}$$
#### ${}^0_{k-1} \mathbf{G}_k \in{\text{SE}(3)}$ and ${}^0_{k-1} \mathbf{G}_k:=\exp{{}^0_{k-1}\mathbf{g}_k}$ with latter se(3)
### **Cost** is
####
$${}^0_{k-1}\mathbf{g}_k^{*\vee}=\argmin\limits_{{}^0_{k-1}\mathbf{g}_k^{*\vee}}{\sum\limits_{i}^{n_d}\rho_h\left( \mathbf{e}_i^{\top}({}^0_{k-1}\mathbf{g}_k)\Sigma_p^{-1}\mathbf{e}_i({}^0_{k-1}\mathbf{g}_k)\right)}$$
#### $n_d$ visible 3D-2D dynamic point correspondences on an object.
## 4. Joint Estimation with Optical Flow
### Refine the estimation of optical flow jointly with the motion estimation
###
