---
title: self-attention
tags: [[sequence-to-sequence]], [[attention]]
---

## The original self-attention paper uses [[LSTM]]
### Long Short-Term Memory-Networks for Machine Reading, EMNLP, 2016.
## Multi-head attention is called self-attention if the input query and the input key are the same
:PROPERTIES:
:id: 602f149c-8acd-4c88-ba97-c4e189bd8f16
:END:
### otherwise, [[cross-attention]]
### self-attention不局限于 [[sequence-to-sequence]]模型，可以用在所有RNN上
## SimpleRNN + Self-Attention
### 计算状态向量
####
$$\mathbf{h}_1=\rm{tanh} \left(\mathbf{A} \cdot \begin{bmatrix} \mathbf{x}_1 \\ \mathbf{c}_0\end{bmatrix} + \mathbf{b}\right)$$
#### 这个$\mathbf{c}_0$就是context vector,具体计算方法参见[[attention]]
#### 对比 {{embed ((602fb278-dac8-4e2f-b66c-5e92a7ce3cc1))}}
####
