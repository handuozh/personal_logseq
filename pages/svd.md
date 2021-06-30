---
title: SVD
alias: singular value decomposition,奇异值分解
tags: [[Matrix]], [[linear algebra]]
---

- based on [[eigenvalue decomposition]]
- 回顾1 对角化分解
    - Given a $m\times m$ matrix $A$ (方阵)，对角化分解
        -
          $$A=U\Lambda U^{-1}$$
            - 其中$U$每一列都是 [[eigenvector]]
            - $\Lambda$对角线上的元素从大到小排列的 [[eigenvalue]]
    - 当$A$是对称矩阵Symmetric时，存在对称对角化分解
        -
          $$A=Q\Lambda Q^{T}$$
            - 其中$Q$每一列都是相互正交的 [[eigenvector]]
                - 而且是单位向量
            - $\Lambda$对角线上的元素从大到小排列的 [[eigenvalue]]
-