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
## Feature distribution
###
