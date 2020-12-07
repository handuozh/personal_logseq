---
title: VDO_SLAM
---

## Notion
### Coordinate Frames
#### Let ${}^0 \mathbf{X}_k,{}^0 \mathbf{L}_k\in{\mathbb{SE}(3)}$ be the robot and object 3D pose respectively, at time $k\in{\mathcal{T}}$ in global reference frame $0$.
### Points in 3D space
#### Let ${}^0 \mathbf{m}_k^i$ be the homogeneous coordinates of the $i^{th}$ 3D point at time $k$ with ${}^0\mathbf{m}^i=\left[m_x^i,m_y^i,m_z^i,1 \right]^{\top} \in{\text{IE}^3}$ and $i\in {\mathcal{M}}$ the set of points.
#### So a point in robot frame as ${}^{X_k}\mathbf{m}_k^i={}^0\mathbf{X}_k^{-1}\cdot  {}^0 \mathbf{m}_k^i$.
#### Define $\mathbf{I}_k$ the reference frame associated with image captured by the camera at time $k$ chosen at the top left corner of the image, and let ${}^{I_k}\mathbf{p}_k^i=\left[u_i,v_i,1 \right]\in{\text{IE}^2}$ be the pixel location on frame $\mathbf{I}_k$ corresponding to the homogeneous 3D point ${}^{\mathbf{X}_k}\mathbf{m}_k^i$, which is obtained via projection function $\pi(\cdot)$
####
$$$$
