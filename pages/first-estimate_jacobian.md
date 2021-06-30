---
title: FIrst-Estimate Jacobian
---

- EKF Linearized Error-State System
    - When developing an extended Kalman filter ([[EKF]]), one needs to linearize the nonlinear motion and measurement models about some linearization point. This linearization is one of the sources of error causing inaccuracies in the estimates (in addition to, for exmaple, model errors and measurement noise). Let us consider the following linearized error-state visual-inertial system:
    -
      $$\tilde{\mathbf{x}}_{k|k-1} = \mathbf{\Phi}_{(k,k-1)}~\tilde{\mathbf{x}}_{k-1|k-1} + \mathbf{G}_{k}\mathbf{w}_{k}$$
        -
          $$\tilde{\mathbf{z}}_{k} = \mathbf{H}_{k}~\tilde{\mathbf{x}}_{k|k-1}+\mathbf{n}_{k}$$
        - where the state contains the inertial navigation state and a single environmental feature 
          (noting that we do not include biases to simplify the derivations):
        -
          $$\mathbf{x}_k =
          	\begin{bmatrix}
          	{}_G^{I_{k}} \bar{q}{}^{\top}
          	& {}^G\mathbf{p}_{{I}_{k}}^{\top}
          	& {}^G\mathbf{v}_{{I}_{k}}^{\top}
          	& {}^G\mathbf{p}_{f}^{\top}
          	\end{bmatrix}^{\top}$$
        -
          #+BEGIN_NOTE
          We use the left quaternion error state 
          #+END_NOTE