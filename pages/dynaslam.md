---
title: DynaSLAM
---

## Keywords
:PROPERTIES:
:heading: true
:END:
### [[Dynamic SLAM]] [[slam+tracking]]
### 评分 [[4💗️]]
## Meta Data
:PROPERTIES:
:heading: true
:END:
### DynaSLAM II: Tightly-Coupled Multi-Object Tracking and SLAM #readdone
### Zotero Metadata

#### * Item Type: [[Article]]
#### * Authors: [[Berta Bescos]], [[Carlos Campos]], [[Juan D. Tardós]], [[José Neira]]
#### 
#### * Date: [[2020-10-15]]
#### [http://arxiv.org/abs/2010.07820](http://arxiv.org/abs/2010.07820)
####  
#### * Cite key: bescosDynaSLAMIITightlyCoupled2020
#### * Topics: [[slam+tracking]], [[dynamic SLAM]]
#### 
#### * Tags: #Computer-Science---Computer-Vision-and-Pattern-Recognition, #Computer-Science---Robotics #zotero #literature-notes #reference

#### PDF Attachments
	- [Bescos et al_2020_DynaSLAM II.pdf](zotero://open-pdf/library/items/L2CWES3T)

#### [[abstract]]:
The assumption of scene rigidity is common in visual SLAM algorithms. However, it limits their applicability in populated real-world environments. Furthermore, most scenarios including autonomous driving, multi-robot collaboration and augmented/virtual reality, require explicit motion information of the surroundings to help with decision making and scene understanding. We present in this paper DynaSLAM II, a visual SLAM system for stereo and RGB-D configurations that tightly integrates the multi-object tracking capability. DynaSLAM II makes use of instance semantic segmentation and of ORB features to track dynamic objects. The structure of the static scene and of the dynamic objects is optimized jointly with the trajectories of both the camera and the moving agents within a novel bundle adjustment proposal. The 3D bounding boxes of the objects are also estimated and loosely optimized within a fixed temporal window. We demonstrate that tracking dynamic objects does not only provide rich clues for scene understanding but is also beneficial for camera tracking. The project code will be released upon acceptance.

#### zotero items: [Local library](zotero://select/items/1_69AP3XQK)
## Notation
:PROPERTIES:
:background_color: #793e3e
:heading: true
:END:
### Camera $i$ has a pose $\mathbf{T}^i_{\rm{CW}}\in \mathbb{SE}(3)$ at time $i$.
### **Static** map points $\mathbf{x}_{\rm{W}}^l \in{\mathbb{R}^3}$
### **Dynamic** objects with index $i$ camera, $k$ object, $j$ point
#### pose $\mathbf{T}^{k,i}_{\rm{WO}} \in {\mathbb{SE}(3)}$
#### linear, angular velocity $\mathbf{v}_i^k,\mathbf{w}_i^k\in{\mathbb{R}^3}$
#### Each observed object $k$ contains dynamic objects points $\mathbf{x}_{\rm{O}}^{j,k}\in{\mathbb{R}^3}$
### [Notation](https://i.imgur.com/0EKVpFo.png){:height 439, :width 480}
## Object Association
:PROPERTIES:
:background_color: #497d46
:heading: true
:END:
### 1. Pixel-wise semantic segmentation and [[ORB]] features
#### If an instance belongs to a dynamic class (car, person, animal)
#### and contains high number of new **nearby** key points
#### new object created -> assign the key points $j$ to the object $k$
### 2. We first associate the **static features** with the ones from the previous frame and the map to initially estimate camera pose.
### 3. **Dynamic features** are associated with the dynamic points from **local map**
#### 3.1 if the velocity of the map objects is known, the matches are searched by [[reprojection]] assuming an inter-frame^^ constant velocity motion^^
#### 3.2 if the objects velocity is not initalized or not enough matches
##### constraint the [[brute force]] matching to the features that belong to the most overlapping instance withint consecutive frames
##### handle occlusion by matching map objects (not previous frame objects)
#### 3.2 higher level association -> [[tracking]] by
##### key points overlapping
##### [[IoU]] of the 2D bounding boxes
### 4. The SE(3) pose of the first object of a track is initialized with the **center of mass** of the 3D points and with identity rotation
#### TODO 这段考虑一下与 [[smoke]] 进行合并分析
:PROPERTIES:
:todo: 1611031368308
:END:
##### ((60065273-b8d6-4dc3-9c7c-a0d07595c3c4))
### 5. Refine the object pose by minimizing [[reprojection]] error
#### Static representation
##### 3D map point $l$ with a stereo key point correspondence $\mathbf{u}^l_i=\left[u,v,u_R\right]\in{\mathbb{R}^3}$ is $$\mathbf{e}_{re}^{i,l}=\mathbf{u}^l_i-\pi_i\left(\mathbf{T}_{\rm{CW}}^i\bar{\mathbf{x}}_{\rm{W}}^l\right)$$
#### ^^Dynamic object error^^
#####
$$\mathbf{e}_{re}^{i,j,k}=\mathbf{u}^j_i-\pi_i\left(\mathbf{T}_{\rm{CW}}^i\mathbf{T}_{\rm{WO}}^{k,i}\bar{\mathbf{x}}_{\rm{O}}^{j,k}\right)$$
##### where $\mathbf{T}_{\rm{WO}}^{k,i}$ is the inverse pose of object $k$ in world coordinate.
### 6. The camera and objects trajectories, as well as the objects bounding
boxes and 3D points are optimized over a sliding window with marginalization and a soft smooth motion prior.
## Object-Centric Representation
:PROPERTIES:
:heading: true
:background_color: #264c9b
:END:
### Reduce amount of parameters to be optimized
#### $N_c$ cameras, $N_o$ dynamic objects with $N_{op}$ 3D points
#### Conventional static SLAM: $N=6N_c + N_o \times 3N_{op}$
#### Total $N=6N_c+N_c \times N_o \times 3N_{op}$ considering each camera
#### If 3D object points become unique to be referred to their object:
##### $N^{\prime}=6N_c +N_c \times 6N_o + N_o \times 3N_{op}$
### 为了降低计算量,每个物体上只考虑一个点,相对这个物体是静止的
####
## [[bundle adjustment]] with Objects
:PROPERTIES:
:heading: true
:background_color: #978626
:END:
### Joint optimization
:PROPERTIES:
:heading: true
:END:
#### [BA factor graph representation with objects](https://i.imgur.com/MxHR7DR.png)
### Key frame insertion condition:
:PROPERTIES:
:heading: true
:END:
#### camera tracking is weak
##### same as conventional static SLAM optimization
##### all the key frames connected to it in the [[covisibility graph]]
#### tracking of any scene object is weak
:PROPERTIES:
:background_color: #793e3e
:END:
##### if an object with a relatively large amount of feautres has few points tracked in the current frame -> create new object with new points
##### local [[BA]] optimizes the pose and velocity of the object and the camera along a^^ temporal tail of 2 seconds^^ together with object points
#### If both object tracking and camera tracking is weak, jointly optimize!
### Smoothing with constant velocity model, object $k$ and observation $i$
#### linear velocity $\mathbf{v}_i^k \in {\mathbb{R}^3}$
#### angular velocity $\mathbf{w}_i^k \in {\mathbb{R}^3}$
#### error term of object velocity
:PROPERTIES:
:heading: true
:END:
#####
$$\mathbf{e}_{\rm{vcte}}^{i,k}=\left( \begin{aligned} \mathbf{v}_{i+1}^k - \mathbf{v}_i^k  \\ \mathbf{w}_{i+1}^k - \mathbf{w}_i^k \end{aligned} \right)$$
#### error term of object transform
:PROPERTIES:
:heading: true
:END:
##### couple object velocity with object poses and corresponding 3D points
######
$$\mathbf{e}_{\rm{vcte,\bf{XYZ}}}^{i,k,k}=\left( \mathbf{T}_{\rm{WO}}^{k,i+1} - \mathbf{T}_{\rm{WO}}^{k,i} \Delta \mathbf{T}_{\rm{O_k}}^{i,i+1} \right) \bar{\mathbf{x}}_{\rm{O}}^{j,k}$$
###### where $\Delta \mathbf{T}_{\rm{O_k}}^{i,i+1}$ is the pose transformation in the time inverval $\Delta t_{i,i+1}$ that object $k$ undergoes.
######
$$\Delta \mathbf{T}_{\rm{O_k}}^{i,i+1}=\left( \begin{aligned} \exp(\mathbf{w}_i^k \Delta t_{i,i+1}) && \mathbf{v}_i^k\Delta t_{i,i+1}  \\ \mathbf{0}_{1\times 3} && 1 \end{aligned} \right)$$
### Joint optimization in the local window $\mathcal{C}$ with each camera $i$ observing a set of map points $\mathcal{MP}_i$ and an object set $\mathcal{O}_i$ containing each object $k$ the set of object points $\mathcal{OP}_k$
####
$$\begin{array}{l}
\min\limits _{\theta} \sum\limits_{i \in \mathcal{C}}\left(\sum\limits_{l \in \mathcal{M} \mathcal{P}_{i}} \rho\left(\left\|\mathbf{e}_{\text {repr }}^{i, l}\right\|_{\Sigma_{i}^{l}}^{2}\right)+\sum\limits_{k \in \mathcal{O}_{i}}\left(\rho\left(\left\|\mathbf{e}_{\text {vete }}^{i, k}\right\|_{\Sigma_{\Delta t}}^{2}\right)\right.\right. \\
\left.\left.+\sum\limits_{j \in \mathcal{O} \mathcal{P}_{k}}\left(\rho\left(\left\|\mathbf{e}_{\text {repr }}^{i, j, k}\right\|_{\Sigma_{i}^{j}}^{2}\right)+\rho\left(\left\|\mathbf{e}_{\text {vcte }, \mathbf{X Y Z}}^{i, j, k}\right\|_{\Sigma_{\Delta t}}^{2}\right)\right)\right)\right)
\end{array}$$
##### for reprojection error $\Sigma$ is covariance matrix (scale of key point observation)
##### for velocity error, $\Sigma$ is associated to time interval.
#### The parameters to be optimized
#####
$$
\theta=\left\{\mathbf{T}_{\mathrm{CW}}^{i}, \mathbf{T}_{\mathrm{W} 0}^{k, i}, \mathbf{X}_{\mathrm{W}}^{l}, \mathbf{X}_{0}^{j, k}, \mathbf{v}_{i}^{k}, \mathbf{w}_{i}^{k}\right\}
$$
## Bounding Boxes
### for [[VDO_SLAM]] and [[ClusterSLAM]]
#### for every dynamic object we estimate by the centroid of map points when **first observed**, like point cloud
### we need to find a ^^common spatial reference^^ for objects of the same semantic class
#### not only dimensions and space occupancy
### Decouple estimation of trajectory and bounding boxes
#### 只考虑每个动态物体上的一个点的6DOF轨迹
#### 这个点是center of mass when first observed
#### 这样两个问题就分解开了,互不干扰
##### dynamic object tracking
##### camera-object view point (point tracking and optimization)
###### 这个点就是那个第一次观测到的点
