---
title: receptive field
alias: 感受野
tags: CNN
---

## In [[CNN]], for any element $x$ of some layer, its _receptive field_ refers to  all the element (from all previous layers)
### that may affect the calculation of $x$ during the forward propogation
### receptive field may be larger than the actual size of the input
## ![image.png](/assets/pages_receptive_field_1611300458316_0.png)
## Given the $2 \times 2$ [[convolution kernel]], the receptive field of the shaded output elements (of value 19) is the four elements in the shaded portion of the input.
:PROPERTIES:
:id: 600a7d0b-da69-463b-9183-3e56d89b77de
:END:
### Denote output matrix $\mathbf{Y}$
### a deeper CNN with additional $2\times 2$ layer with $\mathbf{Y}$ as input
#### output a single element $z$
:PROPERTIES:
:id: 600a7d65-a79c-4c1e-a790-ef1ec76c1116
:END:
### the [[receptive field]] of $z$ on $\mathbf{Y}$ includes **all the four elements of** $\mathbf{Y}$
### the [[receptive field]] of $z$ on the input **includes all the nine elements**.
## Deeper network gives a larger receptive field to detect input features over a broader area.
### 更深的卷积神经网络使特征图中单个元素的感受野变得更加广阔，从而捕捉输入上更大尺寸的特征
