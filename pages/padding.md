---
title: Padding
alias: 填充
tags: CNN, [[convolution kernel]]
---

## ((600a7be5-e4d0-4683-a21a-005a37507974))
### ((600a7c1c-c6bc-4cae-8057-9f227c173c95))
## 在输入的高和宽两端补元素
### 通常补0, 叫做zero padding
### ![image.png](/assets/pages_padding_1611301009617_0.png)
### 如果高的两侧一共填充$p_h$行,宽两侧一共$p_w$行,输出形状为:
####
$$(n_h - k_h +p_h + 1) \times (n_w - k_w +p_w +1)$$
#### 在很多情况下，我们会设置$p_h=k_h -1$ 和$p_w=k_w-1$使输入输出具有相同的高和宽
##### 若$k_h$奇数,在高的两侧分别填充$p_h /2$行,这个比较**常见**
##### 若$k_h$偶数,在顶端一侧填充$\lceil{p_h/2} \rceil$,另一侧$\lfloor{p_h/2} \rfloor$
####
