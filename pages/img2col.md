---
title: img2col
tags: [[convolution kernel]]
---

## 每一步的卷积都提前转换好放到连续的内存中
## 优点: cache性能好
## 缺点: 内存占用大
## Transform the input data of a 3D matrix into a 2D matrix
### height, width, in_channels -> [ height $\times$ width,  k $\times$ k $\times$ in_channels ]
### ![image.png](/assets/pages_img2col_1611283103546_0.png){:height 269, :width 602}
### img2col输出的行数(row) 就是 input data [[convolution kernel]]的移动次数
#### out_height $\times$ out_width
### img2col输出的列数(col) 就是 一个convolution kernel元素的个数
## 这个操作并不是[[reshape]]!
### 当moving step of [[convolution kernel]] 很小,每个kernel会重叠
#### 这时输出2D matrix 通常远远大于输入的3D matrix
#### 冗余内存单元 -> 空间换时间
##
