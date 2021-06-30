---
title: trust region
tags: [[Optimization]]
---

	-
	- Gradient Ascent
		- Problem
			- Find $\mathbf{\theta}^*=\argmax\limits_{\theta} J(\mathbf{\theta})$
		- **Gradient ascent** repreats:
			- At $\theta_{old}$, compute gradient $\mathbf{g}=\frac{\partial J(\theta)}{\partial \theta} \big|_{\theta=\theta_{old}}$
			- Gradient ascent梯度上升: $\theta_{new}\leftarrow \theta_{old} + \alpha \cdot \mathbf{g}$
				- $\alpha$ 学习率
-