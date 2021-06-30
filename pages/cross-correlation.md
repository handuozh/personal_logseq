---
title: cross-correlation
alias: 互相关
tags: [[信号处理]]
---

-
- Given an image $\mathbf{X}\in \mathbb{R}^{M\times N}$ and [[convolution kernel]] $\mathbf{W}\in\mathbb{R}^{U\times V}$
    -
      $$y_{ij}=\sum\limits_{u=1}^{U} \sum\limits_{v=1}^V w_{uv}x_{i+u-1,j+v-1}$$
    - 用$\bigotimes$表示互相关运算, $\mathbf{Y}\in{\mathbb{R}^{M-U+1, N-V+1}}$为输出矩阵
        -
          $$\mathbf{Y=W\bigotimes X}=\text{rot}180(\mathbf{W}) * X$$
- cross-correlation和 [[convolution]] 的区别仅在于 [[convolution kernel]] 是否翻转(flip)
    - 在[[CNN]]中使用 [[convolution]] 是为了[[feature extraction]]
    - 所以cross-correlation和 [[convolution]] 能力上等价的