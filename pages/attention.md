---
title: attention
---

- The original paper
    - Neural machine translation by jointly learning to align and translate, ICLR, 2015.
- RNN + attention
    - does not forget long sequence
        - decoder additionally looks at ^^all the states of the encoder^^
        - 对比standard seq2seq model
            - ((602fb310-8bf7-4748-8f48-d08f07aacc17))
    - [[decoder]] knows where to _focus_
    - But downside is much more computation
    - Simple RNN + attention
      id:: 603070b8-5a03-4cf9-a04b-6defdb8f56e7
        - 计算权重的方法 #related [[transformer]]
        - **weight** $\alpha_i=align(\mathbf{h}_i, \mathbf{s}_0)$
            - $\mathbf{s}_0$与每一个$\mathbf{h}_i$的相关性
        - **Context vector**: $\mathbf{c}_0=\alpha_1 \mathbf{h}_1 + \cdots + \alpha_m \mathbf{h}_m$
            - 1. linear maps
                - $\mathbf{k}_i=\mathbf{W}_K \cdot \mathbf{h}_i$, for $i=1$ to $m$
                - $\mathbf{q}_0=\mathbf{W}_{Q} \cdot \mathbf{s}_0$
            - 2. Inner Product
                - $\tilde{\alpha}=\mathbf{k}_i^{\top}\mathbf{q}_0$ for $i=1$ to $m$
            - 3. Normalization
                - $[\alpha_1, \cdots, \alpha_m]=\rm{Softmax}\left([\tilde{\alpha}_1, \cdots, \tilde{\alpha}_m]\right)$
        - Decoder更新过程
- SimpleRNN的decoder状态向量计算
    - {{embed ((602fb278-dac8-4e2f-b66c-5e92a7ce3cc1)) }}
    - SimpleRNN+attention
                    -
                      $$\mathbf{s}_1=\rm{tanh} \left(\mathbf{A}^{\prime} \cdot \begin{bmatrix} \mathbf{x}^{\prime}_1 \\ \mathbf{s}_0 \\ \mathbf{c}_0\end{bmatrix} + \mathbf{b}\right)$$
                    - 考虑context vector
                - 接着计算decoder状态向量$\mathbf{s}_2$的过程中
                    - ^^重新^^计算权重$\alpha_i$，因为根据定义$h_i$与$s_j$的相关性有不同的权重
                    - 计算加权平均的结果context vector$\mathbf{c}_1$
                    - 计算decoder状态向量$\mathbf{s}_2$
                      id:: 602fb12a-10e7-43e6-b790-bf72c0e70b6b
                - 一个context vector$\mathbf{c}_j$有$m$个权重，decoder有$t$个状态
                    - 所以一共有$mt$个权重
                    - $O(mt)$ time complexity
            - [attention+rnn](https://i.imgur.com/irWmoeR.png)
            -