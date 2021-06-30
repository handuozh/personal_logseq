---
title: GMC
---

- Assumption based on [[GMS]]
- 1. Distance Measurement
  heading:: true
    - Get the 3D point displacement residual
        - between reference frame $\mathcal{F}_{re}$ and  target frame $\mathcal{F}_{ma}$
        -
          $$e_i^j=|| \rm{re}_i - (T_j+R_j \rm{ma}_i)  ||^2 $$
            - $j$ frame index
            - $i$ feature point index
        - Challenge 1
            - Unlike 2D optical flow, 3D Euclidean error triangulated from 2D is impacted by the distance
            - Cannot directly infer full motion
        - Incorporate distance covariance
    - Motion Discretization into a "finite motion category pattern"
        - For next step "consensus"
        -
          #+BEGIN_NOTE
          Motion coherence causes a (small) neighborhood around an object keypoint to drop into the same motion pattern
          #+END_NOTE
- 2. Consensus + Merging
  heading:: true
    -