---
title: Kinematic
---

## Nomenclature 命名
:PROPERTIES:
:heading: true
:background_color: rgb(83, 62, 125)
:END:
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
:PROPERTIES:
:heading: true
:background_color: rgb(73, 125, 70)
:END:
### Cross product / skew
:PROPERTIES:
:heading: true
:END:
####
$$\mathbf{a \times b}=\left[\begin{matrix} a_1 \\ a_2 \\ a_3\end{matrix}\right] \times \left[\begin{matrix} b_1 \\ b_2 \\ b_3\end{matrix}\right]=(\mathbf{a})^{\wedge}\mathbf{b}=\hat{\mathbf{a}}\mathbf{b}=\left[\begin{matrix} 0 & -a_3 & a_2 \\ a_3 & 0 & -a_1 \\ -a_2 & a_1 & 0 \end{matrix} \right]\left[ \begin{matrix} b_1 \\ b_2 \\ b_3\end{matrix}\right]$$
### Unskew
####
$$\mathbf{a}=\hat{\mathbf{a}}^{\vee}, \hat{\mathbf{a}}=-\hat{\mathbf{a}}^{\top}, \mathbf{a\times b}=- \mathbf{b\times a}$$
### Euclidean norm
####
$$||\mathbf{a}||=\sqrt{\mathbf{a^{\top}a}}=\sqrt{a_1^2 + \cdots + a_n^2}$$
### Exponential map for matrix (**exp**)
####
$$\mathbb{R}^{3\times 3} \mapsto \mathbb{R}^{3\times 3}, \mathbf{A} \mapsto e^{\mathbf{A}}, \mathbf{A}\in{\mathbb{R}^{3\times 3}}$$
### Logarithmic map for matrix (**log**)
####
$$\mathbb{R}^{3\times 3} \mapsto \mathbb{R}^{3\times 3}, \mathbf{A} \mapsto \log{\mathbf{A}}, \mathbf{A}\in{\mathbb{R}^{3\times 3}}$$
## Position
:PROPERTIES:
:heading: true
:background_color: rgb(38, 76, 155)
:END:
### Vector
#### $\mathbf{r}_{OP}$, from point $O$ to point $P$
### Position vector
#### ${}_B\mathbf{r}_{OP}$, from point $O$ to point $P$ in frame $B$.
### Homogeneous position vector
#### ${}_B\bar{\mathbf{r}}_{OP}=\left[{}_B\mathbf{r}^{\top}_{OP}, 1\right]^{\top}$, from point $O$ to point $P$ in frame $B$.
## Orientation / Rotation
:PROPERTIES:
:heading: true
:background_color: rgb(121, 62, 62)
:END:
### The most well-known parameterizations are Euler angles, rotation matrix, angle-axis, rotation vector and [[unit quaternion]].
### Active Rotation
:PROPERTIES:
:heading: true
:END:
#### $\Phi^A: {}_I \mathbf{r}_{OP} \mapsto {}_I \mathbf{r}_{OQ}$
#### rotates the vector $\mathbf{r}_{OP}$
### Passive Rotation
:PROPERTIES:
:heading: true
:END:
#### $\Phi^{P}: {}_I\mathbf{r}_{OP} \mapsto {}_B\mathbf{r}_{OP}$
#### rotates the frame ($\mathbf{e}_x^I, \mathbf{e}_y^I, \mathbf{e}_z^I$)
### Elementary Rotations
:PROPERTIES:
:heading: true
:END:
#### ${}_I\mathbf{r}_{OP} = \mathbf{C}_{IB} \cdot {}_B\mathbf{r}_{OP}$
#### [[https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_12_14_t1.png?Expires=4761520316&Signature=VtSP6R552prqdXTO1mMhxajiCuRMpWxga68kGyHSqORsRxxLvYytvs7qJPb~mj1dKzlVqb4bw4tJo5ZDr6FDRqC5dBbMaoie1R4iXLxlpWc~VFurxOxwakAiQXdp2aNcOute2nqgXi606pI9iW9Ac6fCUiy89DiDIZuSNTnAPp~rCCyMKh6Rn6bA5-RFd0snsCgoKNWUInXal~EnHCuO8aO1pWUnp4lbVhMZV3C~7g3KwMz9G-ePiWYFVW2sTlhnXtMCMvRLJ-rNriIivSrF3ddrNZkVPX2fFRI1QNNbfUofW39MLey3b0yT~3FbJqTGoSX9pYyUNeRMVdhWddL2Hw__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA][2020_12_14_t1.png]]
### Inversion
:PROPERTIES:
:heading: true
:END:
#### $\Phi^{A^{-1}}(\mathbf{r})=\Phi^P(\mathbf{r})$
### Concatenation
:PROPERTIES:
:heading: true
:END:
#### $\Phi_2^A\left(\Phi_1^A(\mathbf{r})\right)=\left(\Phi_2^A \otimes \Phi_1^A\right)(\mathbf{r})=\left(\Phi_1^{A^{-1}}\otimes \Phi_2^{A^{-1}} \right)^{-1}(\mathbf{r})$
###
