---
title: invariant representations
---

- [[Fabian B. Fuchs]] #blog
- In machine learning, models need to learn to ignore unimportant or even distracting features.
	- Features that correlate with target labels in the training data but not the test data
	- Trade-off between
		- Sensitivity to relevant features in the data
		- Methods to suppress distractions
			- Model invariance to suppress unimportant or distracting features
- One way of suppressing noisy features in the input is [[regularization]]
- A global symmetry defined as an invertible operator $\mathcal{g}: \mathcal{V}\rightarrow \mathcal{V}$ which, applied to the input of a function $\phi: \mathcal{V}\rightarrow \mathcal{Y}$ does not change its result
	-
	  $$\phi(\mathcal{g} \circ v)=\phi(v)$$
	- If the above hold for invertible operators $\mathcal{g}_1$ and $\mathcal{g}_2$
		- the composition $\mathcal{g}_1 \circ \mathcal{g}_2$ is also an invertible symmetry operator
		- The operators form a group under composition
	-
- [[permutation invariance]]
	- Examples of permutation invariant tasks
		- point cloud classification
		- relational reasoning about multiple objects in an image
		- regression tasks on a group of atoms which together form a molecule
	-