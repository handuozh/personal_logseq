---
title: Measurement Update
public: true
---

## Minimum Mean Square Error (MMSE) Estimation
### Consider the following static state estimation problem:
### Given a prior distribution (probability density function or [[PDF]] ) for a Gaussian random vector
### $\mathbf x\sim \mathcal N (\hat{\mathbf x}^\ominus, \mathbf P_{xx}^\ominus)$ with dimension of $n$ and a new $m$ dimentional measurement $\mathbf{z}_m = \mathbf z + \mathbf n = \mathbf h(\mathbf x) + \mathbf n$ corrupted by zero-mean white Gaussian noise independent of state, $\mathbf n \sim \mathcal N(\mathbf 0, \mathbf R)$,
we want to compute the first two (central) moments of the posterior pdf $p(\mathbf x|\mathbf z_m)$
#### Generally (given a nonlinear measurement model), we approximate the posterior pdf as:
$p(\mathbf x|\mathbf z_m) \simeq \mathcal N (\hat{\mathbf x}^\oplus, \mathbf P_{xx}^\oplus)$.
### By design, this is the (approximate) solution to the MMSE estimation problem [Kay 1993](http://users.isr.ist.utl.pt/~pjcro/temp/Fundamentals%20Of%20Statistical%20Signal%20Processing--Estimation%20Theory-Kay.pdf) @cite Kay1993.
## Conditional Probability Distribution
### [[Bayes Rule]]
### Gaussian [[PDF]]
## 3D Feature [[Triangulation]]
## Camera Measurement Update
### Perspective Projection Measurement Model
### Distortion Model
#### [[pinhole model]]
### Perspective Projection Function
#### The standard pinhole camera model is used to project a 3D point in the camera frame into the normalized image plane (with unit depth):
#####
$$\mathbf{z}_{n,k} = \mathbf h_p  ({}^{C_k}\mathbf{p}_f) =
    \begin{bmatrix}
    {}^Cx/{}^Cz \\
    {}^Cy/{}^Cz
    \end{bmatrix} \\
    \text{where} \quad  {}^{C_k}\mathbf{p}_f = \begin{bmatrix} {}^Cx \\ {}^Cy \\ {}^Cz \end{bmatrix}$$
##### whose Jacobian matrix is computed as follows:
######
### Euclidean Transformation
### [[Point Feature Representation]]
## #flashcardtag
### What is a [[PDF]]?
###
