---
title: convolution kernel
alias: 卷积核
tags: #滤波器, #信号处理
---
## [[cross-correlation]]是一个衡量两个序列[[correlation]]的函数
### 通常用[[sliding window]]的[[点积]]计算实现
## 输出形状由输入形状和kernel窗口形状决定
:PROPERTIES:
:id: 600a7b52-17b9-4f07-a16a-bdf330314886
:END:
### 在2D [[cross-correlation]] operation中滑动
#### [[sliding window]] 从左上角开始,按从左往右、从上往下的顺序，依次在输入数组上滑动
:PROPERTIES:
:id: 600a84d5-8444-43c8-8f1a-1715fa57a135
:END:
### along each axis, the output size is slightly smaller than input size
#### we can only properly compute the [[cross-correlation]] for locations where the kernels ^^fits wholly within the image^^.
### The output size is given by the input size $n_h \times n_w$ minus the size of [[convolution kernel]] $k_h \times k_w$ via
:PROPERTIES:
:id: 600a7be5-e4d0-4683-a21a-005a37507974
:END:
####
:PROPERTIES:
:id: 600a7c1c-c6bc-4cae-8057-9f227c173c95
:END:
$$(n_h - k_h + 1) \times (n_w - k_w +1)$$
#### Two [[hyper-parameters]] to change output size
##### [[Padding]]
##### [[Stride]]
