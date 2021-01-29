---
title: double window optimization
tags: paper, [[Visual SLAM]]
---

## Meta Data
:PROPERTIES:
:heading: true
:END:
### Double window optimisation for constant time visual SLAM #reading
### Zotero Metadata

#### * Item Type: [[Conference paper]]
#### * Authors: [[Hauke Strasdat]], [[Andrew J. Davison]], [[J.M.M. Montiel]], [[Kurt Konolige]]
#### * Proceedings Title: [[2011 International Conference on Computer Vision]]
#### * Date: [[11/2011]]
#### [https://ieeexplore.ieee.org/document/6126517/](https://ieeexplore.ieee.org/document/6126517/)
#### * DOI: [10.1109/ICCV.2011.6126517](https://doi.org/10.1109/ICCV.2011.6126517) 
#### * Cite key: strasdatDoubleWindowOptimisation2011a
#### * Topics: [[feature-based]]
####  #zotero #literature-notes #reference

#### PDF Attachments
	- [Strasdat et al. - 2011 - Double window optimisation for constant time visua.pdf](zotero://open-pdf/library/items/E68TIQEZ)

#### [[abstract]]:
##### We present a novel and general optimisation framework for visual SLAM, which scales for both local, highly accurate reconstruction and large-scale motion with long loop closures. We take a two-level approach that combines accurate pose-point constraints in the primary region of interest with a stabilising periphery of pose-pose soft constraints. Our algorithm automatically builds a suitable connected graph of keyposes and constraints, dynamically selects inner and outer window membership and optimises both simultaneously. We demonstrate in extensive simulation experiments that our method approaches the accuracy of offline bundle adjustment while maintaining constant-time operation, even in the hard case of very loopy monocular camera motion. Furthermore, we present a set of real experiments for various types of visual sensor and motion, including large scale SLAM with both monocular and stereo cameras, loopy local browsing with either monocular or RGB-D cameras, and dense RGB-D object model building.
#### zotero items: [Local library](zotero://select/items/1_LATL9LPQ)
## Related topics: [[relative bundle adjustment]]
## Double window
:PROPERTIES:
:heading: true
:END:
### Inner window of point-pose constraints ( [[BA]] )
#### model the local area **accurately**
### Outer window of pose-pose constraints ( [[Pose graph]] optimization)
#### **stabilize** the periphery, ^^soft constraint^^
#### Pose-pose constraints are defined by [[covisibility graph]]
##### Two poses are connected if sharing enough common features
### Couple the two types of constraints within a single optimization framework
## Graph Structure
:PROPERTIES:
:heading: true
:END:
### keyframe vertices $\mathcal{V}$, a set of 3D points $\mathcal{P}$, and a set of relative edges $\mathcal{E}$
#### Each keyframe vertex $V_i$ saves
##### absolute pose $T_i$
##### which points $\mathbf{x}_k \in \mathcal{P}$ are visible from $T_i$
##### all corresponding observations $\mathbf{z}_{ik}$
#### Edge $E_{ij}$ has covisibility weight $w_{ij}$
##### $w_{ij}$ the number of points are commonly visible in $V_i$ and $V_j$
##### edge also marked marginalized or not
###### If marginalized, stores the relative pose constraint $T_{ij}$
###### Otherwise, the relative pose implicitly defined $T_{ij}=T_i \cdot T_j^{-1}$
### ![image.png](/assets/pages_double_window_optimization_1611634268428_0.png)
### At all times, there is exactly one **reference keyframe** $V_{\rm{ref}}$
## Optimization and Marginalization
### Perform a uniform-cost search over the neighbors of $V_{\rm{ref}}$
#### select the highest covisibility weight $w_{ij}$
#### In contrast to [[geodesic distance]]
### The first $M_1$ keyframes considered as inner window $\mathcal{W}_1$ and following $M_2$ belong to outer window ($M_1 << $M_2$)
### Cost function
#### inner window里面所有的点
#### inner window $\mathcal{W}_1$ 里所有的frames和outer window $\mathcal{W}_2$部分frames 组成point-pose constraints $\mathbf{z}_{ik}$
#### outer window 所有的frames组成pose-pose constraints $T_{ji}$
####
$$
\chi^{2}=\sum_{\mathbf{z}_{i k}}\left(\mathbf{z}_{i k}-\hat{\mathbf{z}}\left(\mathrm{T}_{i}, \mathbf{x}_{k}\right)\right)^{2}+\sum_{\mathrm{T}_{j, i}} v_{j i}^{\top} \Lambda_{\mathrm{T}_{j i}} v_{j i}
$$
##### $v_{ji}:=\log_{SE(3)}(T_{ji}\cdot T_i \cdot T_j^{-1})$ is the relative pose error in the tangent space of SE(3)
##### $\Lambda_{T_{ji}}$ is the **precision matrix** of the binary constraint $T_{ji}$
######
$$
\Lambda_{\mathrm{T}_{j i}}=w_{i j}\left[\begin{array}{cc}
\lambda_{\text {trans }}^{2} \mathrm{I}_{3 \times 3} & \mathrm{O}_{3 \times 3} \\
\mathrm{O}_{3 \times 3} & \lambda_{\text {rot }}^{2} \mathrm{I}_{3 \times 3}
\end{array}\right]
$$
###
