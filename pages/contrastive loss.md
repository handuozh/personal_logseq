---
title: Contrastive Loss
---

## contrastive learning框架有两大目标:

### 不同的原样本有不同的表征，负样本的存在就是为了确保这个目标
### 同一个原样本的不同augmentation结果 / view有相同的表征
## Contrastive loss function
:PROPERTIES:
:id: 602dca30-8789-4c00-8cc9-b034eccd08d8
:END:
###
$$\mathcal{L}(A,B)=y^*|| f(A)-f(B)||^2+(1-y^*)\max (0, m^2-|| f(A)-f(B)||^2)$$
##
