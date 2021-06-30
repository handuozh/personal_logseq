---
title: normalization
alias: 标准化
---

- [[preprocessing]]
    - 标准化后,把不同图片**映射到同一个坐标系,同一个尺度**
        - 一定程度上消除因为过度曝光,质量不佳或者噪声等对模型权值更新的影响
    - 过大像素值分量(RGB)主导权值更新的问题 转为 RGB分量都具有相同的*数值分布*
        - 一定程度上消除gradient更新的收敛慢,无序的问题
- 分类
  heading:: true
    - [[min-max normalization]]
    - [[z-score normalization]]
    -