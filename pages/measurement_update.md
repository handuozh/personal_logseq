---
title: Measurement Update
---

## Minimum Mean Square Error (MMSE) Estimation
## Conditional Probability Distribution
### [[Bayes Rule]]
### Gaussian [[PDF]]
## 3D Feature [[Triangulation]]
## Camera Measurement Update
### Perspective Projection Measurement Model
### Distortion Model
### Perspective Projection Function
#### The standard pinhole camera model is used to project a 3D point in the camera frame into the normalized image plane (with unit depth):
#####
$$\mathbf{z}_{n,k} = \mathbf h_p  ({}^{C_k}\mathbf{p}_f) =
    \begin{bmatrix}
    {}^Cx/{}^Cz \\
    {}^Cy/{}^Cz
    \end{bmatrix} \\
    \text{where} \quad  {}^{C_k}\mathbf{p}_f = \begin{bmatrix} {}^Cx \\ {}^Cy \\ {}^Cz \end{bmatrix}$$
##### whose Jacobian matrix is computed as follows:
######
### Euclidean Transformation
### [[Point Feature Representation]]
###
