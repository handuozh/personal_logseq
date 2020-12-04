---
title: Point Feature Representation
public: true
---
##
---
alias: 特征点参数化, [[feature parametrization]]
---
## There are two main parameterizations of a 3D point feature: 3D position (xyz) and inverse depth with bearing.
### Both of these can either be represented in the global frame (全局坐标系) or in an anchor frame (局部相机坐标系) of reference which adds a dependency on having an "anchor" pose where the feature is observed.
### To allow for a unified treatment of different feature parameterizations $\boldsymbol \lambda$ in our codebase, we derive in detail the generic function ${}^{G}\mathbf{p}_f=\mathbf f (\cdot)$ that maps different representations into global position.
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
### The global inverse-depth representation of a 3D point feature is given by　（akin to spherical coordinates):
####
$${}^{G}\mathbf{p}_f
= \mathbf f(\boldsymbol\lambda) \\
= \frac{1}{\rho}\begin{bmatrix} \cos(\theta)\sin(\phi) \\ \sin(\theta)\sin(\phi) \\ \cos(\phi) \end{bmatrix} \\
\text{where} \quad \boldsymbol\lambda = \begin{bmatrix} \theta  & \phi & \rho \end{bmatrix}^\top$$
### The Jacobian w.r.t feature parameters:
####
$$\frac{\partial \mathbf f(\cdot)}{\partial \boldsymbol\lambda} = 
\begin{bmatrix}
-\frac{1}{\rho}\sin(\theta)\sin(\phi) & \frac{1}{\rho}\cos(\theta)\cos(\phi) & -\frac{1}{\rho^2}\cos(\theta)\sin(\phi) \\
\frac{1}{\rho}\cos(\theta)\sin(\phi) & \frac{1}{\rho}\sin(\theta)\cos(\phi) & -\frac{1}{\rho^2}\sin(\theta)\sin(\phi) \\
0 & -\frac{1}{\rho}\sin(\phi) & -\frac{1}{\rho^2}\cos(\phi)
\end{bmatrix}$$
## 3. Anchored XYZ
:PROPERTIES:
:heading: true
:END:
### We can represent a 3D point feature in some "anchor" frame (say some IMU local frame, $\{{}^{I_a}_{G}\mathbf{R},~{}^{G}\mathbf{p}_{I_a}\}$), which would normally be the IMU pose corresponding to the first camera frame where the feature was detected.
####
$${}^{G}\mathbf{p}_f
= \mathbf f(\boldsymbol\lambda,~{}^{I_a}_{G}\mathbf{R},~{}^{G}\mathbf{p}_{I_a},~{}^{C}_{I}\mathbf{R},~{}^{C}\mathbf{p}_{I}) \\
=
{}^{I_a}_{G}\mathbf{R}^\top{}^{C}_{I}\mathbf{R}^\top( \boldsymbol\lambda -{}^{C}\mathbf{p}_{I}) + {}^{G}\mathbf{p}_{I_a} \\
\text{where} \quad \boldsymbol\lambda = {}^{C_a}\mathbf{p}_f = \begin{bmatrix} {}^{C_a}x & {}^{C_a}y & {}^{C_a}z \end{bmatrix}^\top$$
### The Jacobian w.r.t feature state is given by
####
$$\frac{\partial \mathbf f(\cdot)}{\partial \boldsymbol\lambda} = {}^{I_a}_{G}\mathbf{R}^\top{}^{C}_{I}\mathbf{R}^\top
$$
### As the anchor pose is involved in the representation, its Jacobians are:
####
$$\frac{\partial \mathbf f(\cdot)}{\partial {}^{I_a}_{G}\mathbf{R}} = -{}^{I_a}_{G}\mathbf{R}^\top \left\lfloor{}^{C}_{I}\mathbf{R}^\top({}^{C_a}\mathbf{p}_f-{}^{C}\mathbf{p}_{I}) \times\right\rfloor \\
    \frac{\partial \mathbf f(\cdot)}{\partial {}^{G}\mathbf{p}_{I_a}} = \mathbf{I}_{3\times 3}$$
#### Moreover, if performing extrinsic calibration, the following Jacobians with respect to the IMU-camera extrinsics are also needed:
#####
$$
 \frac{\partial \mathbf f(\cdot)}{\partial {}^{C}_{I}\mathbf{R}} = -{}^{I_a}_{G}\mathbf{R}^\top{}^{C}_{I}\mathbf{R}^\top \left\lfloor({}^{C_a}\mathbf{p}_f-{}^{C}\mathbf{p}_{I}) \times\right\rfloor \\
    \frac{\partial \mathbf f(\cdot)}{\partial {}^{C}\mathbf{p}_{I}} = -{}^{I_a}_{G}\mathbf{R}^\top{}^{C}_{I}\mathbf{R}^\top
$$
## 4. Anchored Inverse Depth
:PROPERTIES:
:heading: true
:END:
### In analogy to the global inverse depth case, we can employ the inverse-depth with bearing (akin to spherical coordinates) in the anchor frame, $\{{}^{I_a}_{G}\mathbf{R},~{}^{G}\mathbf{p}_{I_a}\}$, to represent a 3D point feature:
####
$$
{}^{G}\mathbf{p}_f
= \mathbf f(\boldsymbol\lambda,~{}^{I_a}_{G}\mathbf{R},~{}^{G}\mathbf{p}_{I_a},~{}^{C}_{I}\mathbf{R},~{}^{C}\mathbf{p}_{I}) \\
= {}^{I_a}_{G}\mathbf{R}^\top{}^{C}_{I}\mathbf{R}^\top({}^{C_a}\mathbf{p}_f-{}^{C}\mathbf{p}_{I}) + {}^{G}\mathbf{p}_{I_a} \\
=
{}^{I_a}_{G}\mathbf{R}^\top{}^{C}_{I}\mathbf{R}^\top\Bigg(\frac{1}{\rho}\begin{bmatrix} \cos(\theta)\sin(\phi) \\ \sin(\theta)\sin(\phi) \\ \cos(\phi) \end{bmatrix}-{}^{C}\mathbf{p}_{I}\Bigg) + {}^{G}\mathbf{p}_{I_a} \\
\text{where} \quad \boldsymbol\lambda = \begin{bmatrix} \theta & \phi & \rho \end{bmatrix}^\top
$$
### The Jacobian w.r.t. the feature state is given by:
####
$$\frac{\partial \mathbf f(\cdot)}{\partial \boldsymbol\lambda} = 
{}^{I_a}_{G}\mathbf{R}^\top{}^{C}_{I}\mathbf{R}^\top
\begin{bmatrix}
-\frac{1}{\rho}\sin(\theta)\sin(\phi) & \frac{1}{\rho}\cos(\theta)\cos(\phi) & -\frac{1}{\rho^2}\cos(\theta)\sin(\phi) \\
\frac{1}{\rho}\cos(\theta)\sin(\phi) & \frac{1}{\rho}\sin(\theta)\cos(\phi) & -\frac{1}{\rho^2}\sin(\theta)\sin(\phi) \\
0 & -\frac{1}{\rho}\sin(\phi) & -\frac{1}{\rho^2}\cos(\phi)
\end{bmatrix}$$
### The Jacobian w.r.t. anchor pose are:
####
$$\frac{\partial \mathbf f(\cdot)}{\partial {}^{I_a}_{G}\mathbf{R}} = -{}^{I_a}_{G}\mathbf{R}^\top \left\lfloor{}^{C}_{I}\mathbf{R}^\top({}^{C_a}\mathbf{p}_f-{}^{C}\mathbf{p}_{I}) \times\right\rfloor \\
    \frac{\partial \mathbf f(\cdot)}{\partial {}^{G}\mathbf{p}_{I_a}} = \mathbf{I}_{3\times 3}$$
### The Jacobian w.r.t. IMU-camera extrinsics are:
####
$$\frac{\partial \mathbf f(\cdot)}{\partial {}^{C}_{I}\mathbf{R}} = -{}^{I_a}_{G}\mathbf{R}^\top{}^{C}_{I}\mathbf{R}^\top \left\lfloor({}^{C_a}\mathbf{p}_f-{}^{C}\mathbf{p}_{I}) \times\right\rfloor \\
    \frac{\partial \mathbf f(\cdot)}{\partial {}^{C}\mathbf{p}_{I}} = -{}^{I_a}_{G}\mathbf{R}^\top{}^{C}_{I}\mathbf{R}^\top$$
## 5. Anchored Inverse Depth ([[MSCKF]] Version)
:PROPERTIES:
:heading: true
:END:
### Note that a simpler version of inverse depth was used in the original [[MSCKF]] paper @cite Mourikis2007ICRA. This representation does not have the singularity if it is represented in a camera frame the feature was measured from.
####
$${}^{G}\mathbf{p}_f
= \mathbf f(\boldsymbol\lambda,~{}^{I_a}_{G}\mathbf{R},~{}^{G}\mathbf{p}_{I_a},~{}^{C}_{I}\mathbf{R},~{}^{C}\mathbf{p}_{I})  \\
=
{}^{I_a}_{G}\mathbf{R}^\top{}^{C}_{I}\mathbf{R}^\top({}^{C_a}\mathbf{p}_f-{}^{C}\mathbf{p}_{I}) + {}^{G}\mathbf{p}_{I_a} \\
=
{}^{I_a}_{G}\mathbf{R}^\top{}^{C}_{I}\mathbf{R}^\top\Bigg(\frac{1}{\rho}\begin{bmatrix} \alpha \\ \beta \\ 1 \end{bmatrix}-{}^{C}\mathbf{p}_{I}\Bigg) + {}^{G}\mathbf{p}_{I_a} \\
\text{where} \quad \boldsymbol\lambda = \begin{bmatrix} \alpha & \beta & \rho \end{bmatrix}^\top$$
### The Jacobian w.r.t. the feature state is given by:
####
$$\frac{\partial \mathbf f(\cdot)}{\partial \boldsymbol\lambda} = 
{}^{I_a}_{G}\mathbf{R}^\top{}^{C}_{I}\mathbf{R}^\top
\begin{bmatrix}
\frac{1}{\rho} & 0 & -\frac{1}{\rho^2}\alpha \\
0 & \frac{1}{\rho} & -\frac{1}{\rho^2}\beta \\
0 & 0 & -\frac{1}{\rho^2}
\end{bmatrix}$$
### The Jacobians w.r.t. anchor state are:
####
$$\frac{\partial \mathbf f(\cdot)}{\partial {}^{I_a}_{G}\mathbf{R}} = -{}^{I_a}_{G}\mathbf{R}^\top \left\lfloor{}^{C}_{I}\mathbf{R}^\top({}^{C_a}\mathbf{p}_f-{}^{C}\mathbf{p}_{I}) \times\right\rfloor \\
    \frac{\partial \mathbf f(\cdot)}{\partial {}^{G}\mathbf{p}_{I_a}} = \mathbf{I}_{3\times 3}$$
### The Jacobian w.r.t. IMU-camera extrinsics are:
####
$$\frac{\partial \mathbf f(\cdot)}{\partial {}^{C}_{I}\mathbf{R}} = -{}^{I_a}_{G}\mathbf{R}^\top{}^{C}_{I}\mathbf{R}^\top \left\lfloor({}^{C_a}\mathbf{p}_f-{}^{C}\mathbf{p}_{I}) \times\right\rfloor \\
    \frac{\partial \mathbf f(\cdot)}{\partial {}^{C}\mathbf{p}_{I}} = -{}^{I_a}_{G}\mathbf{R}^\top{}^{C}_{I}\mathbf{R}^\top$$
## 6. Anchored Inverse Depth ([[MSCKF]] Single Depth Version)
:PROPERTIES:
:heading: true
:END:
### This feature representation is based on the [[MSCKF]] representation @cite Mourikis2007ICRA, and the the single depth from [[VINS-Mono]] @cite Qin2018TRO.
#### As compared to the implementation in @cite Qin2018TRO, we are careful about how we handle treating of the bearing of the feature.
### During initialization we initialize a full 3D feature and then follow that by marginalize the bearing portion of it leaving the depth in the state vector. The marginalized bearing is then fixed for all future linearizations.
### Then during update, we perform nullspace projection at every timestep to remove the feature dependence on this bearing.
#### To do so, we need at least *two* sets of UV measurements to perform this bearing nullspace operation since we loose two dimensions of the feature in the process.
We can define the feature measurement function as follows.
####
$${}^{G}\mathbf{p}_f
= \mathbf f(\boldsymbol\lambda,~{}^{I_a}_{G}\mathbf{R},~{}^{G}\mathbf{p}_{I_a},~{}^{C}_{I}\mathbf{R},~{}^{C}\mathbf{p}_{I})  \\
=
{}^{I_a}_{G}\mathbf{R}^\top{}^{C}_{I}\mathbf{R}^\top({}^{C_a}\mathbf{p}_f-{}^{C}\mathbf{p}_{I}) + {}^{G}\mathbf{p}_{I_a} \\
=
{}^{I_a}_{G}\mathbf{R}^\top{}^{C}_{I}\mathbf{R}^\top\Big(\frac{1}{\rho}\hat{\mathbf{b}}-{}^{C}\mathbf{p}_{I}\Big) + {}^{G}\mathbf{p}_{I_a} \\
\text{where} \quad \boldsymbol\lambda = \begin{bmatrix} \rho \end{bmatrix}$$
#### In the above case we have defined a bearing $\hat{\mathbf{b}}$ which is the marginalized bearing of the feature after initialization.
After collecting two measurement, we can nullspace project to remove the Jacobian in respect to this bearing variable.
####
