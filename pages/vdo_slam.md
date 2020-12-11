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
#### ^^(7)^^  $$\mathbf{e}_i({}^0 \mathbf{X}_k)={}^{I_k}\tilde{\mathbf{P}}_k^i-\pi\left({}^0\mathbf{X}_k^{-1} \cdot {}^0\mathbf{m}_{k-1}^i\right)$$
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
#### ^^(10)^^   $$\mathbf{e}_i({}^0_{k-1} \mathbf{H}_k)\begin{aligned} &={}^{I_k}\tilde{\mathbf{P}}_k^i-\pi\left({}^0\mathbf{X}_k^{-1} \cdot {}^0_{k-1} \mathbf{H}_k \cdot {}^0 \mathbf{m}_{k-1}^i \right) \\ &={}^{I_k}\tilde{\mathbf{P}}_k^i-\pi\left( {}^0_{k-1} \mathbf{G}_k \cdot {}^0 \mathbf{m}_{k-1}^i\right)\end{aligned}$$
#### ${}^0_{k-1} \mathbf{G}_k \in{\text{SE}(3)}$ and ${}^0_{k-1} \mathbf{G}_k:=\exp{{}^0_{k-1}\mathbf{g}_k}$ with latter se(3)
### **Cost** is
#### ^^(11)^^  $${}^0_{k-1}\mathbf{g}_k^{*\vee}=\argmin\limits_{{}^0_{k-1}\mathbf{g}_k^{*\vee}}{\sum\limits_{i}^{n_d}\rho_h\left( \mathbf{e}_i^{\top}({}^0_{k-1}\mathbf{g}_k)\Sigma_p^{-1}\mathbf{e}_i({}^0_{k-1}\mathbf{g}_k)\right)}$$
#### $n_d$ visible 3D-2D dynamic point correspondences on an object.
## 4. Joint Estimation with Optical Flow
### Refine the estimation of optical flow jointly with the motion estimation
### (2)+(7) =>
#### ^^(12)^^  $$\mathbf{e}_i({}^0 \mathbf{X}_k, {}^{I_k}\phi)={}^{I_{k-1}}\tilde{\mathbf{P}}_{k-1}^i+{}^{I_k}\phi^i-\pi\left({}^0\mathbf{X}_k^{-1} \cdot {}^0\mathbf{m}_{k-1}^i\right)$$
##### where $$\mathbf{e}_i({}^{I_k}\phi^i)={}^{I_k}\hat{\phi}^i-{}^{I_k}\phi^i$$
### Minimizing the **camera pose** cost with [[Lie-algebra]] parameterisation of SE(3) element:
#### ^^(14)^^ 
\begin{aligned}
\left\{{ }^{0} \mathbf{x}_{k}^{* \vee},{ }^{k} \boldsymbol{\Phi}_{k}^{*}\right\}=& \underset{\left\{{{}^0\mathbf{x}}_{k}^{\vee},{ }^{k}{\boldsymbol{\Phi}}_{k}\right\}}{\operatorname{argmin}} \sum_{i}^{n_{b}}\left\{\rho_{h}\left(\mathbf{e}_{i}^{\top}\left({ }^{I_{k}} \boldsymbol{\phi}^{i}\right) \Sigma_{\phi}^{-1} \mathbf{e}_{i}\left({ }^{I_{k}} \boldsymbol{\phi}^{i}\right)\right)\right. + \\
&\left.\rho_{h}\left(\mathbf{e}_{i}^{\top}\left({ }^{0} \mathbf{x}_{k},{ }^{I_{k}} \boldsymbol{\phi}^{i}\right) \Sigma_{p}^{-1} \mathbf{e}_{i}\left({{}^0\mathbf{x}}_{k},{ }^{I_{k}} \boldsymbol{\phi}^{i}\right)\right)\right\}
\end{aligned}
##### where $\rho_{h}\left(\mathbf{e}_{i}^{\top}\left({ }^{0} \mathbf{x}_{k},{ }^{I_{k}} \boldsymbol{\phi}^{i}\right) \Sigma_{p}^{-1} \mathbf{e}_{i}\left({\mathbf{x}}_{k},{ }^{I_{k}} \boldsymbol{\phi}^{i}\right)\right)$ is the [[regularization]] term.
##### Here $${}^{I_k}\hat{\Phi}^i=\{{}^{I_k}\hat{\phi}^i | i\in{\mathcal{M}},k\in{\mathcal{T}}\}$$ is the initial optic-flow.
### For **object motion** cost:
#### ^^(15)^^ 
$$\begin{aligned}
\left\{{ }^{0}_{k-1} \mathbf{g}_{k}^{* \vee},{ }^{k} \boldsymbol{\Phi}_{k}^{*}\right\}=& \underset{\left\{{{}^0_{k-1}\mathbf{g}}_{k}^{\vee},{ }^{k}{\boldsymbol{\Phi}}_{k}\right\}}{\operatorname{argmin}} \sum_{i}^{n_{d}}\left\{\rho_{h}\left(\mathbf{e}_{i}^{\top}\left({ }^{I_{k}} \boldsymbol{\phi}^{i}\right) \Sigma_{\phi}^{-1} \mathbf{e}_{i}\left({ }^{I_{k}} \boldsymbol{\phi}^{i}\right)\right)\right. + \\
&\left.\rho_{h}\left(\mathbf{e}_{i}^{\top}\left({ }^{0}_{k-1} \mathbf{g}_{k},{ }^{I_{k}} \boldsymbol{\phi}^{i}\right) \Sigma_{p}^{-1} \mathbf{e}_{i}\left({{}^0_{k-1}\mathbf{g}}_{k},{ }^{I_{k}} \boldsymbol{\phi}^{i}\right)\right)\right\}
\end{aligned}$$
## 4. Graph Optimization
:PROPERTIES:
:background_color: rgb(121, 62, 62)
:heading: true
:END:
### [[factor graph]]
### Joint optimization of 3D point measurements, VO measurements, motion of points on dynamic object, object smooth motion observations.
### [[https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_12_10_factor%20graph%20of%20object-aware%20slam.png?Expires=4761190329&Signature=V70cE14yCCGcGrWcptKJgkYDxlJGTlkCnYt8OGb4D40sC-MtlPauxpcnNhCgIRzWGCo~HcoTZWI3RvB1RJHdH4sJK4Y31DA32fqtXtGpN64hmU4S993kqkmgknB2kY9LhaNKJhQ4qXhGsY4xXoMd6b085tCMS9P5MlIl-W~d1~YrkdGVNNqHywXFb85VsSErsy3Vcnt3H9tFlWqgnBp-gxZQt~6SfWdCq~uzKllKEOrSdSEla4Iuas0mXyRBttgFH4c9Y9nDdfnmK9aW61eOrBv8hzBQeT-aa69ylIj8nHiSSCbiq9D0cVTN67t5AUp-MFNgo3W9kCOEvTtiOAxlJA__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA][2020_12_10_factor graph of object-aware slam.png]]
### 4.1 3D point measurement model error $\mathbf{e}_{i,k}({}^0\mathbf{X}_k,{}^0\mathbf{m}_k^i)$
:PROPERTIES:
:heading: true
:END:
#### ^^(16)^^ 
$$
\mathbf{e}_{i, k}\left({ }^{0} \mathbf{X}_{k},{ }^{0} \mathbf{m}_{k}^{i}\right)={{}^0\mathbf{X}}_{k}^{-1}{ }^{0} \mathbf{m}_{k}^{i}-\mathbf{z}_{k}^{i}
$$
##### here $\mathbf{z}=\{\mathbf{z}_k^i | i\in{\mathcal{M}},k\in{\mathcal{T}}\}$ is the set of all 3D point measurements at all time steps, with **cardinality** $n_z$.
##### Shown in white circles.
### 4.2 Visual Odometry model error $\mathbf{e}_k({}^0\mathbf{X}_{k-1},{}^0\mathbf{X}_{k})$
:PROPERTIES:
:heading: true
:END:
#### ^^(17)^^ 
$$\mathbf{e}_k({}^0\mathbf{X}_{k-1},{}^0\mathbf{X}_{k})=({}^0\mathbf{X}_{k-1},{}^0\mathbf{X}_{k})^{-1} {}_{k-1}^{X_{k-1}} \mathbf{T}_k$$
##### where $\mathbf{T}=\{{}_{k-1}^{X_{k-1}} \mathbf{T}_k | k\in{\mathcal{T}}\}$ is odometry measurement set SE(3) and **cardinality** $n_o$.
##### Shown in orange circles.
### 4.3 Motion model error $\mathbf{e}_{i,l,k}({}^0\mathbf{m}_k^i,{}^0_{k-1}\mathbf{H}_k^l,{}^0\mathbf{m}_{k-1}^i)$
:PROPERTIES:
:heading: true
:END:
#### ^^(18)^^
$$\mathbf{e}_{i,l,k}({}^0\mathbf{m}_k^i,{}^0_{k-1}\mathbf{H}_k^l,{}^0\mathbf{m}_{k-1}^i)={}^0\mathbf{m}_k^i - {}^0_{k-1}\mathbf{H}_k^l,{}^0\mathbf{m}_{k-1}^i$$
#### Shown in magenta circles, is a ternary factor as _motion model of a point on a rigid body_.
####
