---
title: Point Feature Representation
:alias: 特征点参数化
:public: true
---
## There are two main parameterizations of a 3D point feature: 3D position (xyz) and inverse depth with bearing.
### Both of these can either be represented in the global frame or in an anchor frame of reference which adds a dependency on having an "anchor" pose where the feature is observed.
### To allow for a unified treatment of different feature parameterizations $\boldsymbol \lambda$ in our codebase, 
we derive in detail the generic function ${}^{G}\mathbf{p}_f=\mathbf f (\cdot)$ that maps different representations into global position.
### Both of these can either be represented in the global frame or in an anchor frame of reference which adds a dependency on having an "anchor" pose where the feature is observed.
To allow for a unified treatment of different feature parameterizations $\boldsymbol \lambda$ in our codebase, 
we derive in detail the generic function ${}^{G}\mathbf{p}_f=\mathbf f (\cdot)$ that maps different representations into global position.
## 1. Global XYZ
:PROPERTIES:
:heading: true
:END:
### As the canonical parameterization, the global position of a 3D point feature is simply given by its xyz coordinates in the global frame of  reference:
####
$${}^{G}\mathbf{p}_f
= \mathbf f(\boldsymbol\lambda) \\
= \begin{bmatrix} {}^Gx \\ {}^Gy \\ {}^Gz \end{bmatrix} \\
\text{where} \quad \boldsymbol\lambda = {}^{G}\mathbf{p}_f = \begin{bmatrix} {}^Gx & {}^Gy & {}^Gz \end{bmatrix}^\top
$$
#### It is clear that the Jacobian with respect to the feature parameters is:
$$
\frac{\partial \mathbf f(\cdot)}{\partial \boldsymbol\lambda} = \mathbf{I}_{3\times 3}
$$
## 2. Global Inverse Depth
:PROPERTIES:
:heading: true
:END:
###