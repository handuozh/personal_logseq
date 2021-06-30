---
title: Triangulation
---

- 1. Linear Triangulation
  heading:: true
    - We wish to create a solvable linear system that can give us an initial guess for the 3D position of our feature. To do this, we take all the poses that the feature is seen from to be of known quantity.
    - This feature will be triangulated in some anchor camera frame $\{A\}$ which we can arbitrary pick. If the feature $\mathbf{P}_f$ is observed by pose $1\cdots m$ , given the anchor pose $A$, we can have transfromation from any camera pose $C_i,i=1\cdots m$:
        -
          $${}^{C_i}\mathbf{p}_{f} = {}^{C_i}_A\mathbf{R} \left( {}^A\mathbf{p}_f - {}^A\mathbf{p}_{C_i}\right) \\
          {}^A\mathbf{p}_f = {}^{C_i}_A\mathbf{R}^\top {}^{C_i}\mathbf{p}_{f} + {}^A\mathbf{p}_{C_i}
          $$
    - In the absents of noise, the measurement in the current frame is the unknown bearing ${}^{C_i}\mathbf{b}$ and its depth ${}^{C_i}z$.
    - Thus we have the following mapping to a feature seen from the current frame:
        -
          $${}^{C_i}\mathbf{p}_f
          	= {}^{C_i}z_{f} {}^{C_i}\mathbf{b}_{f}
          	= {}^{C_i}z_{f}
          	\begin{bmatrix}
          	u_n \\ v_n \\ 1
          	\end{bmatrix}$$
    - We note that $u_n$ and $v_n$ represent the undistorted normalized image coordinates.
    - This bearing can be warped into the the anchor frame by substituting into the above equation:
        -
          $${}^A\mathbf{p}_f
          = {}^{C_i}_A\mathbf{R}^\top z_{f} {}^{C_i}\mathbf{b}_{f} + {}^A\mathbf{p}_{C_i} \\
          = z_{f} {}^{A}\mathbf{b}_{C_i \rightarrow f} + {}^A\mathbf{p}_{C_i}$$
    - To remove the need to estimate the extra degree of freedom of depth $z_{f}$, we define the following two vectors:
        -
          $${}^{A}\mathbf{n}_{1} = \begin{bmatrix} -{}^{A}{b}_{C_i \rightarrow f}(3) & 0 & {}^{A}{b}_{C_i \rightarrow f}(1) \end{bmatrix}^{\top} \\
            {}^{A}\mathbf{n}_{2} = \begin{bmatrix} 0 & {}^{A}{b}_{C_i \rightarrow f}(3) & -{}^{A}{b}_{C_i \rightarrow f}(2) \end{bmatrix}^{\top}
          $$
    - These are perpendicular with the vector ${}^{A}\mathbf{b}_{C_i \rightarrow f}$ and thus ${}^{A}\mathbf{n}_{1}^{\top}{}^{A}\mathbf{b}_{C_i \rightarrow f}=0$ and ${}^{A}\mathbf{n}_{2}^{\top}{}^{A}\mathbf{b}_{C_i \rightarrow f}=0$ holds true.
    - We can then multiple the transform equation/constraint to form two equation which only relates to the unknown 3 d.o.f ${}^A\mathbf{p}_f$:
        -
          $$\begin{bmatrix}
          	{}^{A}\mathbf{n}_{1}^{\top} \\
          	{}^{A}\mathbf{n}_{2}^{\top} 
          	\end{bmatrix}
          	{}^A\mathbf{p}_f = 
          	\begin{bmatrix}
          	{}^{A}\mathbf{n}_{1}^{\top} \\
          	{}^{A}\mathbf{n}_{2}^{\top} 
          	\end{bmatrix}
          	z_{f} {}^{A}\mathbf{b}_{C_i \rightarrow f} +
          	\begin{bmatrix}
          	{}^{A}\mathbf{n}_{1}^{\top} \\
          	{}^{A}\mathbf{n}_{2}^{\top} 
          	\end{bmatrix}
          	{}^A\mathbf{p}_{C_i} \\
          	\begin{bmatrix}
            {}^{A}\mathbf{n}_{1}^{\top} \\
            {}^{A}\mathbf{n}_{2}^{\top} 
            \end{bmatrix}
            {}^A\mathbf{p}_f =
            \begin{bmatrix}
            {}^{A}\mathbf{n}_{1}^{\top} \\
            {}^{A}\mathbf{n}_{2}^{\top} 
            \end{bmatrix}
            {}^A\mathbf{p}_{C_i}$$
    - By stacking all the measurements, we can have:
        -
          $$\begin{bmatrix}
          	\vdots 
          	\\
          	\begin{bmatrix}
          	{}^{A}\mathbf{n}_{1}^{\top} \\
          	{}^{A}\mathbf{n}_{2}^{\top} 
          	\end{bmatrix}
          	\\
          	\vdots
          	\end{bmatrix}
          	{}^A\mathbf{p}_f =  
          	\begin{bmatrix}
          	\vdots 
          	\\
          	\begin{bmatrix}
          	{}^{A}\mathbf{n}_{1}^{\top} \\
          	{}^{A}\mathbf{n}_{2}^{\top} 
          	\end{bmatrix}
          	{}^A\mathbf{p}_{C_i}
          	\\
          	\vdots
          	\end{bmatrix}$$
    - Since each pixel measurement provides two constraints, as long as $m>1$, we will have enough constraints to triangulate the feature. 
      In practice, the more views of the feature the better the triangulation and thus normally want to have a feature seen from at least five views.
      We additionally check that the triangulated feature is "valid" and in front of the camera and not too far away.
    - The [condition number](https://en.wikipedia.org/wiki/Condition_number) of the above linear system and reject systems that are "sensitive" to errors and have a large value.
- 2. Non-linear Feature Optimization
    - After we get the triangulated feature 3D position, a nonlinear least-squares will be performed to refine this estimate.
    - In order to achieve good numerical stability, we use the inverse depth representation for point feature which helps with convergence.
    - We find that in most cases this problem converges within 2-3 iterations in indoor environments.
    - The feature transformation can be written as:
        -
          $${}^{C_i}\mathbf{p}_f = 
          	{}^{C_i}_A\mathbf{R}
          	\left(
          	{}^A\mathbf{p}_f - {}^A\mathbf{p}_{C_i}
          	\right) \\
          	= 
          	{}^Az_f
          	{}^{C_i}_A\mathbf{R}
          	\left(
          	\begin{bmatrix}
          	{}^Ax_f/{}^Az_f  \\ {}^Ay_f/{}^Az_f \\ 1
          	\end{bmatrix}
          	-
          	\frac{1}{{}^Az_f}
          	{}^A\mathbf{p}_{C_i}
          	\right)
          	\\ 
          	\Rightarrow
          	\frac{1}{{}^Az_f}
          	{}^{C_i}\mathbf{p}_f
          	= 
          	{}^{C_i}_A\mathbf{R}
          	\left(
          	\begin{bmatrix}
          	{}^Ax_f/{}^Az_f  \\ {}^Ay_f/{}^Az_f \\ 1
          	\end{bmatrix}
          	-
          	\frac{1}{{}^Az_f}
          	{}^A\mathbf{p}_{C_i}
          	\right)$$
    - We define $u_A = {}^Ax_f/{}^Az_f$, $v_A = {}^Ay_f/{}^Az_f$, and $\rho_A = {1}/{{}^Az_f}$ to get the following measurement equation:
        -
          $$h(u_A, v_A, \rho_A)
          	= 
          	{}^{C_i}_A\mathbf{R}
          	\left(
          	\begin{bmatrix}
          	u_A  \\ v_A \\ 1
          	\end{bmatrix}
          	-
          	\rho_A
          	{}^A\mathbf{p}_{C_i}
          	\right)$$
    - The feature measurement seen from the $\{C_i\}$ camera frame can be reformulated as:
        -
          $$\mathbf{z}  
          	= 
          	\begin{bmatrix}
          	u_i \\ v_i
          	\end{bmatrix} \\
                = 
          	\begin{bmatrix}
          	h(u_A, v_A, \rho_A)(1)  / h(u_A, v_A, \rho_A)(3) \\
          	h(u_A, v_A, \rho_A)(2)  / h(u_A, v_A, \rho_A)(3) \\
          	\end{bmatrix}
          	\\
          	= 
          	\mathbf{h}(u_A, v_A, \rho_A)$$
    - Therefore, we can have the least-squares formulated and Jacobians:
        -
          $$	\operatorname*{argmin}_{u_A, v_A, \rho_A}||{\mathbf{z} - \mathbf{h}(u_A, v_A, \rho_A)}||^2
          $$
        -
          $$\frac{\partial \mathbf{h}(u_A, v_A, \rho_A)}{\partial {h}(u_A, v_A, \rho_A)}
            = 
            \begin{bmatrix}
            1/h(\cdots)(1) & 0 & -h(\cdots)(1)/h(\cdots)(3)^2 \\
            0 & 1/h(\cdots)(2) & -h(\cdots)(2)/h(\cdots)(3)^2
            \end{bmatrix} \\
          	\frac{\partial {h}(u_A, v_A, \rho_A)}{\partial [u_A, v_A, \rho_A]}
             = 
            {}^{C_i}_A\mathbf{R}
            \begin{bmatrix}
            \begin{bmatrix}
            1 & 0 \\
            0 & 1 \\
            0 & 0
            \end{bmatrix} & -{}^A\mathbf{p}_{C_i}
            \end{bmatrix}$$
    - The least-squares problem can be solved with [Gaussian-Newton](https://en.wikipedia.org/wiki/Gauss%E2%80%93Newton_algorithm) or [Levenberg-Marquart](https://en.wikipedia.org/wiki/Levenberg%E2%80%93Marquardt_algorithm) algorithm.