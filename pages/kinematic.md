---
title: Kinematic
---

## Nomenclature 命名
### Identity matrix: $\mathbb{1}_{n\times m}$
### Coordinate system
#### $\mathbf{e}_x^A, \mathbf{e}_y^A, \mathbf{e}_z^A$
#### Cartesian right-hand system A with basis (unit) vectors $\mathbf{e}$
### Inertial frame
#### $\mathbf{e}_x^I, \mathbf{e}_y^I, \mathbf{e}_z^I$
#### global / inertial / world coordinate system
### Body-fixed frame
#### $\mathbf{e}_x^B, \mathbf{e}_y^B, \mathbf{e}_z^B$
#### local / body-fixed coordinate system (moves with body)
### Rotation
#### $\Phi \in{SO(3)}$
#### generic rotation (for all parameterizations)
## Operators
### Cross product / skew / unskew
####
$$\mathbf{a \times b}=\left[\begin{matrix} a_1 \\ a_2 \\ a_3\end{matrix}\right] \times \left[\begin{matrix} b_1 \\ b_2 \\ b_3\end{matrix}\right]=(\mathbf{a})^{\wedge}\mathbf{b}=\hat{\mathbf{a}}\mathbf{b}=\left[\begin{matrix} 0 -a_3 a_2 \\ a_3 0 -a_1 \\ -a_2 a_1 0 \end{matrix} \right]\left[ \begin{matrix} b_1 \\ b_2 b_3\right]\end{matrix}$$
####
