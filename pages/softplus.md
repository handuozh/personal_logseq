---
title: softplus
type: [[activation function]] 
---

- $f(x)=\ln (1+\exp(x))$
    - 可以看作 [[ReLU]] 的平滑
- ![image.png](../assets/pages_softplus_1616669735353_0.png)
- Derivative
    - $f^{\prime}(x)=\exp(x) / (1+\exp(x))=1/(1+\exp(-x))$
    - [[logistic function]]