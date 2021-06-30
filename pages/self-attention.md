---
title: self-attention
tags: [[sequence-to-sequence]], [[attention]]
---

- The original self-attention paper uses [[LSTM]]
    - Long Short-Term Memory-Networks for Machine Reading, EMNLP, 2016.
- Multi-head attention is called self-attention if the input query and the input key are the same
  id:: 602f149c-8acd-4c88-ba97-c4e189bd8f16
    - otherwise, [[cross-attention]]
    - self-attention不局限于 [[sequence-to-sequence]]模型，可以用在所有RNN上
- SimpleRNN + Self-Attention
    - 计算状态向量
        -
          $$\mathbf{h}_1=\rm{tanh} \left(\mathbf{A} \cdot \begin{bmatrix} \mathbf{x}_1 \\ \mathbf{c}_0\end{bmatrix} + \mathbf{b}\right)$$
        - 这个$\mathbf{c}_0$就是context vector,具体计算方法参见[[attention]]
        - 对比 {{embed ((602fb278-dac8-4e2f-b66c-5e92a7ce3cc1))}}
- self-attention与[[CNN]]之间的区别 [[Permanent notes]] 
  heading:: true
    - {{embed ((60487e34-e0ba-45d2-a926-42c8fce3208c)) }}
    - self-attention中每个滑动窗中每个特征变换的参数是共享的
        - 这里的滑动窗大小为整个feature map，即全局
        - 特征之间的相关性是通过某种度量（比如 [[dot product]] 点积）来建模的
    - self-attention, [[Non-local]] networks和 [[GCN]] 可以通过如下统一起来
        - ^^(1)^^            $y_i=\frac{1}{C(x)} \sum\limits_{\forall j} f(x_i, x_j)g(x_j)$
            - 其中$x_i \in \mathbb{R}^{f_{in}\times 1}$，是$i$这个node的特征
            - $f(x_i, x_j)\in \mathbb{R}^0$ 度量两个node的similarity
            - $C(x)\in \mathbb{R}^0$归一化分母
            - $g(x)\in \mathbb{R}^{f_{out}\times 1}$空间变换
            - $y_i \in \mathbb{R}^{f_{out}\times 1}$输出
                - $f_{in}$输入特征通道数
                - $f_{out}$输出特征通道数
        - **self-attention**里
            - $g(x_j)=W_g x_j$
            - $f(x_i,x_j)=x_i^{\top}W_{\theta}^{\top}W_{\phi}x_j$
                - $W_g,W_{\theta},W_{\phi}\in \mathbb{R}^{f_{out}\times f_{in}}$
        - [[Non-local]] 里
            - $f(x_i,x_j)=e^{x_i^{\top}W_{\theta}^{\top}W_{\phi}x_j}$
        - [[GCN]]里
            - $f(x_i,x_j)$定义为$i$和$j$两个node之间有边
    - [[CNN]]可以表示为
        - ^^(2)^^          $y_i=\sum\limits_{j=0}^{L-1} W_j x_{i+j-\frac{L}{2}}=\sum\limits_{j=0}^{L-1} g_j(x_{i+j-\frac{L}{2}})$
            - where $L$ is the length of convolutional kernel
    - (1) 中每个点的空间变换是相同的, (2) 中不同点的空间变换不同
    - (1) 中通过$f(x_i,x_j)$对每个空间变换后的特征进行加权
        - 显式地对node与node之间的相关性进行了建模
        - 而且这个相关性对于输入而言是动态dynamic的
    - CNN需要叠很多层filters，才可以看到长时的信息
        - 而self-attention可以直接考虑整体，同时可以并行化
    -