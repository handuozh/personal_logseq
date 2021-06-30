---
title: dilated conv
alias: 空洞卷积,膨胀卷积,atrous conv
source: https://zhuanlan.zhihu.com/p/50369448
---

- FCN先像传统的CNN那样对图像做卷积再pooling，降低图像尺寸的同时增大感受野
	- 由于图像分割预测是pixel-wise的输出，所以要将pooling后较小的图像尺寸upsampling到原始的图像尺寸进行预测
		- upsampling一般采用[[deconv]]反卷积操作，deconv可参见CVPR2016 Shallow and Deep Convolutional Networks for Saliency Prediction (https://www.zhihu.com/question/43609045/answer/132235276)
	- 之前的pooling操作使得每个pixel预测都能看到较大感受野信息
		- ![no_padding_strides.gif](/assets/pages_convolution_1611297903090_0.gif){:height 177, :width 158}
	- 因此图像分割FCN中有两个关键
		- pooling减小图像尺寸增大感受野
		- upsampling扩大图像尺寸
	- pooling 的主要缺陷
	  heading:: true
		- 参数不可学习
			- Up-sampling / pooling layer (e.g. [[bilinear interpolation]] ) is deterministic.
		- 丢失细节信息,精度下降 (为了增加 [[receptive field]])
			- 内部数据结构丢失
			- 空间层级化信息丢失
		- 小物体信息无法重建
			- 假设有四个pooling layer 则 任何小于 2^4 = 16 pixel 的物体信息将理论上无法重建
	- 在先减小再增大尺寸的过程中，肯定有一些信息损失掉了那么能不能设计一种新的操作，不通过pooling也能有较大的感受野看到更多的信息呢？答案就是[[dilated conv]]
- 定义
  heading:: true
	- Dilated Convolutions are a type of convolution that “**inflate**” the kernel by inserting **holes** between the kernel elements. **空洞**
		- An additional $l$ parameter  (**dilation rate**) indicates how much the kernel is widened.
		- There are usually $l-1$  spaces inserted between kernel elements.
	-
	- ![2020_12_28_compare.png](https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_12_28_compare.png?Expires=4762720900&Signature=M4xjtTMZ7NxBI6vyb7G80VOTwRoKe3wqLbizMzLHQfvbz6W6O8Gtc4rbHRXoKDZxhzk5vN54E25~N~mfICtkMGNQoED8VJIbdYTj3s6ZP2NMbth5epQgfB4-lEuMmYJ3kCFAApgvioyW4WzqWJGGqOl8suW5dbVpRT8pkmRMdz8aIP9pGrriSJ8WndwHY4Hs9H5e3z40skWGwBHQDObxsknzuycqLlOlFkqOfbkU0yRcvNjX~FQJJEKugSePgzyo~22fwEpCE7pwAuIbK6evUVGVeaJpwiWjh4kOK5A1SWxtH6S-15HbmLaGUwWl87RJQMFrpcvZPrCUegjYndkHvg__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA){:height 265, :width 749}
	- ![dilation.gif](/assets/pages_dilated_conv_1611304238899_0.gif){:height 197, :width 205}
	- 优点
	  heading:: true
		- 增加 [[receptive field]] 的同时不增加额外参数
		- 捕获**多尺度**的context上下文信息
			- dilation rate的参数可以设置不同的 [[receptive field]]
	- 缺点
	  heading:: true
		- The Gridding Effect
			- 网格效应/棋盘效应
			- 假设我们仅仅多次叠加 dilation rate 2 的 3 x 3 kernel 的话，则会出现这个问题
				- 邻近的像素从相互独立的subset中卷积得到,互相缺少依赖
				- ![2020_12_28_gridding.jpg](https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_12_28_gridding.jpg?Expires=4762721737&Signature=g5RywuH5voI1c8d~0KQ9pgK1blP1AYo2kHFwr2I-DzCx0Y-oRFkDOIv~v2XcUtEsctTOsKtH2exPHG8THArIG6MRVJb8YJcg~e~WR-QTwnCTNCRyhe4w4d4mKXEaby~~3XzH37T3kUlyxlBSloCLEn9r6PtqeIuO~43h6A63U6Ul6ufdar97wNewnQ-ccw4KGNosPsTzeEloKVj9aTNCkP7XlUrBvgxbQDYxHM~izIMcidkZp-~T~wyLaz-F7OV23BT2J50lfqWKFMVIwZMawijW~8j-PmWJFWk600omTLNZwSQVLDAPU1sW~t5ZxsqCMb2TkSPhSVdNrBdf06DSlw__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA){:height 233, :width 702}
			- **局部信息丢失**
				- kernel 并不连续，也就是并不是所有的 pixel 都用来计算了，因此这里将信息看做 checker-board 的方式会损失信息的连续性
				- 该层的卷积结果间没有相关性,局部信息丢失
				- 这对 pixel-level dense prediction 的任务来说是致命的
			- **远距离获取的信息没有相关性**
				- Long-ranged information might be not relevant
				- 光采用大 dilation rate 的信息或许只对一些大物体分割有效果，而对小物体来说可能则有弊无利了
			- 解决方式
				- [[Hybrid Dilated Convolution]] #toread  #HDC
				- [[dilated residual networks]] #toread
		- 损失了pooling的invariance
			- 抗攻击效果变差
-