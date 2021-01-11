---
title: cross-entropy
alias: 交叉熵
---

## 信息论中cross-entropy定义
###
$$-\int{p(x)\log{g(x)}dx}$$
### 描述两个[[distribution]]的距离
### 训练的目的是使$g(x)$逼近$p(x)$
#### [[softmax]]中$g(x)$是最后一层$y$输出,$p(x)$是[[one-hot]]标签
#### [[Sigmoid]]中最后一层输出相加不为1,所以不能看作一个[[分布]]
##### 将最后一层的每个[[神经元]]看作一个分布,对应的target属于[[二项分布]],target值表示这个类的概率
##### 所有[[神经元]]累加起来
## Given a sample $\{x, t\}$,两种cross-entropy区别在于对应的输出层
### $-t_j \log{y_j}$
#### where $t_j$说明sample的ground truth是第j类
#### 对应的最后一层是 [[softmax]]
### $-\sum_i t_i \log{y_i}+(1-t_i)\log{1-y_i}$
#### 对应的最后一层是 [[Sigmoid]]
##
