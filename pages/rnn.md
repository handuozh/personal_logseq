---
title: RNN
---

## SimpleRNN
### 计算SimpleRNN的decoder状态向量
:PROPERTIES:
:id: 602fb278-dac8-4e2f-b66c-5e92a7ce3cc1
:END:
####
$$\mathbf{s}_1=\rm{tanh} \left(\mathbf{A}^{\prime} \cdot \begin{bmatrix} \mathbf{x}^{\prime}_1 \\ \mathbf{s}_0\end{bmatrix} + \mathbf{b}\right)$$
#### 把新的输入$\mathbf{x}_1$与旧的状态向量$\mathbf{s}_0$做concatenation
## SimpleRNN的decoder状态向量计算
:PROPERTIES:
:id: 602fac8d-85a0-4297-a5e7-48680d5f11e6
:END:
###
$$\mathbf{s}_1=\rm{tanh} \left(\mathbf{A}^{\prime} \cdot \begin{bmatrix} \mathbf{x}^{\prime}_1 \\ \mathbf{s}_0 \end{bmatrix} + \mathbf{b}\right)$$
###
