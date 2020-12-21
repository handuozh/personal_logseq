---
title: MOTS
---

### Abstract
:PROPERTIES:
:heading: true
:END:
#### Multi-object Tracking and Segmentation
#### #code https://www.vision.rwth-aachen.de/page/mots
#### Segmentation vs. Bounding boxes
##### [[https://i.imgur.com/8h8HIgI.png][Segmentation vs. Bounding boxes]]
#### When objects pass each other, large parts of an object’s bounding box may belong to another instance, while per-pixel segmentation masks locate objects precisely.
#### Propose two datasets with instance segmentation labels
##### [[KITTI]]
##### [[MOTChallenge]] dataset
#### Propose new measurement metrics
##### soft Multi-Object Tracking and Segmentation Accuracy
##### [[TrackR-CNN]]
### Evaluation Measures
#### TODO  read on!
:PROPERTIES:
:todo: 1608522599059
:END:
#### Ground truth of a video with $T$ time frames, height $h$, and width $w$ consists of a set of $N$ non-empty gt pixel masks $M=\{m_1,\cdots, m_N\}$ with $m_i\in{\{0,1\}^{h\times w}}$
##### each belongs to a corresponding time frame $t_m \in{\{1,\cdots, T\}}$ and is assigned to a gt track id $\text{id}_m \in {\mathbb{N}}$.
#### The output of a MOTS is a set of $K$ non-empty hpyothesis masks $H=\{h_1,\cdots, h_k\}$ with $h_i\in{\{0,1\}^{h\times w}}$
##### each assigned to a hypothesized track id $\text{id}_h \in {\mathbb{N}}$ and a time frame $t_h \in {\{1,\cdots, T\}}$.
#### Establishing Correspondences
#### Mask-based Evaluation Measures
##### MOTSP
##### sMOTSA
### Method
:PROPERTIES:
:heading: true
:END:
#### Overview
##### [[Mask R-CNN]] plus an association head and two 3D convolutional layers.
##### [[https://i.imgur.com/b0RZUlH.png][TrackR-CNN Overview]]
#### Integrating temporal context
#### Association Head
#### Mask Propagation
