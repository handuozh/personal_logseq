---
title: DISK
---

## Meta Data
:PROPERTIES:
:heading: true
:END:
### #title DISK: Learning local features with policy gradient, 2020
### #topic [[Reinforcement Learning]], [[Keypoint]]
## Abstract
:PROPERTIES:
:heading: true
:END:
### DISK for _DIScrete Keypoints_
### based on ^^discreteness^^ inherent to the selection and matching of sparse features.
### Probabilistic model
## Notion
:PROPERTIES:
:heading: true
:END:
### Given images $A$ and $B$, extract a set of local features $F_A$, $F_B$ and match to get a set of correspondences $M_{A\leftrightarrow B}$.
### $P(F_I|I,\theta_F)$ distribution over sets of $F_1$, cond. on feature detection params $\theta_F$.
### $P(M_{A\leftrightarrow B}|F_A,F_B,\theta_M)$ is distribution over matches between images $A$ and $B$, cond. on matching params $\theta_M$.
### Solution: estimate gradients of expected reward $\Delta_{\theta} \mathbb{E}_{M_{A\leftrightarrow B}\sim P(M_{A\leftrightarrow B}|A,B,\theta)}R(M_{A\leftrightarrow B})$ vias [[Monte Carlo]] sampling and use gradient ascent to maximize the quantity.
## Feature distribution $P(F_I | I, \theta_F)$
### Feature extraction is based on [[U-Net]] with 1 channel for detection and $N$ for description, as $\mathbf{K}$ and $\mathbf{D}$. $F=\{K, D\}$, $N=128$.
### $\mathbf{K}$ subdivided into a grid with cell size $h\times h$, similar to [[Superpoint]].
#### Crop feature map to cell $u$, denoted as $\mathbf{K}^u$ and use [[softmax]] to normalize it.
#### 信tm就连分组都不愿意出，就tm只有置顶和不置顶，每次都tm得在几百个人里面找，这tm废物软件我真的不知道是那帮人带头用来办公的
