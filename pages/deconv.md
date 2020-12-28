---
title: deconv
---

## deconv的其中一个用途是做upsampling，即增大图像尺寸.
### 而dilated conv并不是做upsampling，而是增大感受野。
### 可以形象的做个解释：对于标准的k*k卷积操作，stride为s，分三种情况:
#### 1. s>1，即卷积的同时做了downsampling，卷积后图像尺寸减小
#### 2. s=1，普通的步长为1的卷积，比如在tensorflow中设置padding=SAME的话，卷积的图像输入和输出有相同的尺寸大小
#### 0<s<1，fractionally strided convolution，相当于对图像做upsampling
##### 比如s=0.5时，意味着在图像每个像素之间padding一个空白的像素后，stride改为1做卷积，得到的feature map尺寸增大一倍
##### 而dilated conv不是在像素之间padding空白的像素，而是在已有的像素上，skip掉一些像素，或者输入不变，对conv的kernel参数中插一些0的weight，达到一次卷积看到的空间范围变大的目的。当然将普通的卷积stride步长设为大于1，也会达到增加感受野的效果，但是stride大于1就会导致downsampling，图像尺寸变小
