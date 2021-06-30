---
title: Stride
alias: 步幅
tags: CNN, [[convolution kernel]]
---

- ((600a84d5-8444-43c8-8f1a-1715fa57a135))
- 将每次滑动的行数和列数成为步幅stride
- ![image.png](/assets/pages_stride_1611302193196_0.png)
- 高上stride $s_h$, 宽上stride $s_w$,输出形状为:
    -
      $$\lfloor{(n_h - k_h +p_h + s_h)/s_h}\rfloor \times \lfloor{(n_w - k_w +p_w +s_w)/s_w} \rfloor$$
    - ![image.png](/assets/pages_stride_1611303389399_0.png){:height 95, :width 673}
    -