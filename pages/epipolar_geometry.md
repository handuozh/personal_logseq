---
title: epipolar geometry
alias: 对极几何
---

- 用于匹配特征点,如果匹配成功,说明这两个特征点是同一个空间点在两个成像平面上的投影.
-
  #+BEGIN_QUOTE
  The epipolar geometry between two views is essentially the geometry of the intersection of the image planes with the pencil of planes having the baseline as axis 
  (the baseline is the line joining the camera centres). 
  #+END_QUOTE
- Point correspondence geometry
  heading:: true
    - ![image.png](/assets/pages_epipolar_geometry_1612509591157_0.png){:height 340, :width 775}
        - (a) Two cameras indicated by centers $\mathbf{C}$ and $\mathbf{C}^{\prime}$ and image planes
        - (b) An image point $\mathbf{x}$ back-projects to a ray in 3D space defined by $\mathbf{C}$ and $\mathbf{x}$.
            - 两条epipolar line 极线
            - The ray is imaged as a line $\mathbf{l}^{\prime}$ in second view
            - The 3D space point $\mathbf{X}$ must lie on the ray
- Epipolar geometry
  heading:: true
    - ![image.png](/assets/pages_epipolar_geometry_1612509853623_0.png)
        - (a) **epipoles**极点 $\mathbf{e,e^{\prime}}$
        - (b) As the position of 3D point $\mathbf{X}$ varies, the epipolar planes "rotate" about the baseline.
            - The family of planes is an **epipolar pencil**, which intersect at the epipole.