---
title: Ceres Solver
---

- #topic #Optimization
- 两部分
	- Problem的构建，换句话说就是数据的管理，优化的参数，优化的const function
	- Problem 的求解，就是Hessian矩阵的构建
		- [[Evaluator]]
		- [[Covariance]] estimation
			- ceres的协方差估计支持两种参数空间的协方差估计ambient space和 tangent space
			- 前者就是对global parameter 的协方差估计，后者就是对local parameter 参数的估计
			- 我们通过将problem传递给covariance, 然后调用`GetCovarianceBlockInTangentSpace`计算`local_parameter1_`,`local_parameter2`参数块 tangent space的协方差
			-
			  ```C++
			  Covariance covariance(options);
			  covariance.Compute(covariance_blocks, &problem_);
			  MatrixXd actual(local_parameter1_size, local_parameter2_size);
			  covariance.GetCovarianceBlockInTangentSpace(local_parameter1_address,
			                                            local_parameter1_address,
			                                            actual.data());
			  ```
		-
-