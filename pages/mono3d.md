---
title: mono3D
---

-
  #+BEGIN_QUOTE
  Monocular 3D object detection methods can be roughly divided into two categories by the type of training data
  #+END_QUOTE
    - 1. uses complex features, such as [[instance segmentation]], vehicle shape prior and even depth map to select best proposals in multi-stage fusion module
        - 速度慢
        - need additional annotation work to train some standalone networks
    - 2. employs 2D bounding box and properties of a 3D object as [[supervised]] data.
        - build a deep regression network to predict directly 3D information.
        - Then apply geometric constraints from 3D box vertexes to 2D box edges to refine or predict parameters.
            - 4 edges -> 4 constraints 太少
            - each vertex of a 3D bounding box might correspond to any edges
            - slight error in 2D box causes sharp decline in 3D prediction performance