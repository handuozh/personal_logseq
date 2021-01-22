---
title: homogeneous coordinate
---

## #[[multi-view geometry]] 

## A point in 3D-space is represented in homogeneous coordinates as a 4-vector.
### the homogeneous vector $\mathbf{x}=(x_1,x_2, x_3,x_4)^{\top}$
### with $x_4\neq 0$ represents the point $(x,y,z)^{\top}$ of $\mathbb{R}^3$ with inhomogeneous coordinates
####
$$x=x_1/x_4, y=x_2/x_4, z=x_3/x_4$$
### So a homogeneous representation of $(x,y,z)^{\top}$ is $\mathbf{x}=(x,y, z,1)^{\top}$.
### $x_4=0$ represents points at ^^infinity^^.
## A** [[projective transformation]]** acting on $\mathbb{P}^3$ is a [[linear transform]] on homogeneous 4-vectors represented by a non-singular $4\times 4$ matrix
### $\mathbf{x}^{\prime}=H\mathbf{x}$
### $H$ is homogeneous and has ^^15^^ [[degrees of freedom]]
#### The [[dof]] follow the 16 elements of the matrix less one for overall scaling.
## Planar projective transformation
### the map is a [[collineation]]
####
