---
title: Triangulation
---

## Linear Triangulation
### We wish to create a solvable linear system that can give us an initial guess for the 3D position of our feature. To do this, we take all the poses that the feature is seen from to be of known quantity.
### This feature will be triangulated in some anchor camera frame $\{A\}$ which we can arbitrary pick. If the feature $\mathbf{P}_f$ is observed by pose $1\cdot m$ , given the anchor pose $A$,
###
