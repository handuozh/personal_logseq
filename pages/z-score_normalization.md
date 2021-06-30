---
title: z-score normalization
alias: standardization
---

- 将数据按照行或列或其他属性值减去其均值再除以标准差
    - 得到的数据都聚集在0附近,标准差为1,符合[[norm distribution]]
    -
      $$x^* = \frac{x-\mu}{\sigma}$$
    - 常用
    - 这种方法跟 [[正态分布]] 无关,任何分布都可以有[[z-score normalization]]
- 实现 in python
    - `sklearn.preprocessing.scale()`
    - `sklearn.preprocessing.StandardScaler`类
    -