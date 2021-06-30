---
title: l1 loss
---

- l1 正则先验分布
    - 从[[Maximum a posteriori estimation]]的角度
    - 在知道了$\mathbf{X},y$后,需要对参数w进行建模
        -
          $$MAP=\log P(y|\mathbf{X}, \omega)P(\omega)=\log P(y|\mathbf{X}, \omega)+\log P(\omega)$$
        - 看到后验概率函数在[[似然函数]]基础上增加了$\log P(\omega)$
            - $P(\omega)$是对权重系数w的概率分布的先验假设
            - 可根据w在$\mathbf{X},y$的后验概率对w进行修正
        - 假设w的先验分布为0均值的 [[Gaussian distribution]]
            - $\omega \sim N(0, \sigma^2)$
            -
              $$\begin{aligned} \log P(\omega) &= \log \prod\limits_j P(\omega_j) \\ &= \log \prod\limits_j \left[ \frac{1}{\sqrt{2\pi}\sigma} e^{-\frac{(\omega_j)^2}{2\sigma^2}}\right] \\ &= -\frac{1}{2\sigma^2}\sum\limits_j {\omega_j}^2 + C \end{aligned}$$
            - 可以看出, [[Gaussian distribution]]下$\log P(\omega)$等价于增加 [[L2-norm]]正则项.
        - 假设w的先验分布服从均值为0,参数为$a$的[[Laplacian distribution]]
            - $P(\omega_j)=\frac{1}{\sqrt{2a}} e^{\frac{|\omega_j|}{a}}$
            -
              $$\begin{aligned} \log P(\omega) &= \log \prod\limits_j P(\omega_j) \\ &= \log \prod\limits_j \left[ \frac{1}{\sqrt{2a}\sigma} e^{-\frac{|\omega_j|}{a}}\right] \\ &= -\frac{1}{2a}\sum\limits_j |\omega_j| + C \end{aligned}$$
            - 可以看出, [[Laplacian distribution]]下$\log P(\omega)$等价于代价函数中增加L1正则项.