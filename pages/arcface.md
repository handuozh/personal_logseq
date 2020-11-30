---
title: ArcFace
---

## Abstract
### 在[[SphereFace]]基础上改进了对特征向量归一化和加性角度间隔,提高了类间可分性同时加强类内紧度和类间差异
### 对特征向量和权重归一化，对$\theta$加上角度间隔$m$，角度间隔比余弦间隔在对角度的影响更加直接。几何上有恒定的线性角度margin
#### ArcFace中是直接在角度空间$\theta$中最大化分类界限，而[[CosFace]]是在余弦空间$\cos(\theta)$中最大化分类界限
### 训练（人脸分类器）：ResNet50 + ArcFace loss
### 测试：从人脸分类器FC1层的输出中提取512维的嵌入特征，对输入的两个特征计算余弦距离，再来进行人脸验证和人脸识别
## References:
### https://zhuanlan.zhihu.com/p/76541084
## 4 Kinds of Geodesic Distance (GDis) constraint
### ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FSLAM%2FiDlS4jGqkv.png?alt=media&token=e4962c77-861d-498f-8d9e-55237abc4c73)
### (A) Margin-Loss
### (B) Intra-Loss
### (C) Inter-Loss
### (D) Triplet-Loss
### (E) **Additive Angular Margin Loss (ArcFace)**
## Network Architecture
### ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FSLAM%2F3vCP8VbDkt.png?alt=media&token=1b529fdc-6bd9-45aa-b271-4c45cf51cabc)
## 对比
### ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FSLAM%2F-2CQgZ9fIj.png?alt=media&token=404cac07-7afc-415e-8211-cbd25d01cf38)
## Pseudo Code
### 对$$x$$进行归一化
### 对 $$W$$ 进行归一化
### 计算 $$W_x$$ 得到预测向量$$y$$
### 从 $$y$$中挑出与ground truth对应的值
### 计算其反余弦得到角度
### 角度加上m
### 得到挑出从$$y$$ 中挑出与ground truth对应的值所在位置的one-hot code
- 将$$cos(\theta+m)$$通过独热码放回原来的位置
- 对所有值乘上固定值$$s$$.
