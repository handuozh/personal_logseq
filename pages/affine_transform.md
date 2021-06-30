---
title: affine transform
alias: 仿射变换
---

- “线性变换”+“平移”
    - map a circle to an ellipse
    - cannot map an ellipse to a hyperbola or parabola
    -
- 线性变换 linear transform
    - 从几何上三个性质
        - 变换前是直线的，变换后依然是直线
        - 直线比例保持不变
            - the ratio of lengths on parallel line segments is an [[invariant]]
            - the ratio of two lengths that are not parallel is not
        - 变换前是原点的，变换后依然是原点
    - 一个vector $\mathbf{A}=[x,y]^{\top}$
        - 旋转矩阵$\mathbf{T}_{rotate}=\begin{bmatrix} \cos{\theta} & -\sin{\theta} \\ \sin{\theta} & \cos{\theta} \end{bmatrix}$
        - $\mathbf{A}^{\prime} = \mathbf{T}_{rotate} \cdot \mathbf{A}$
        - 线性变换通过**矩阵乘法**实现
- 平移后原点改变,线性变换->仿射变换 (增加一个维度)
    -
      $$\mathbf{y}=\mathbf{Ax+b}$$
    -
      $$\begin{bmatrix} \mathbf{y} \\ \mathbf{1} \end{bmatrix}=\begin{bmatrix} \mathbf{A} & \mathbf{b} \\ \mathbf{0} & 1 \end{bmatrix} \begin{bmatrix} \mathbf{x} \\ 1 \end{bmatrix}$$
    - 增加一个维度后,可以在^^高维度^^通过[[linear transform]]实现^^低维度^^的仿射变换