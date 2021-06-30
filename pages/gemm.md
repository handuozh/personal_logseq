---
title: GEMM
---

- General matrix multiplication, one of the basic [[linear algebra]] subprograms
- 解决大矩阵，快速operation
- ![image.png](/assets/pages_gemm_1611282697774_0.png)
-
- [[convolution]] operation
-
- [[img2col]] 转换成连续存储
- 简单的矩阵乘法, $\mathcal{O}(n^3)=\mathcal{O}(n^{\log_2(8)})$
- 分块乘法 (4块)
- [[Strassen]]算法, $\mathcal{O}(n^{2.807})=\mathcal{O}(n^{\log_2(7)})$
- [[Coppersmith-Winograd]]算法, $\mathcal{O}(n^{2.38})$