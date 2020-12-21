---
title: MOTS
public: true
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
:PROPERTIES:
:heading: true
:background_color: rgb(121, 62, 62)
:END:
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
:background_color: rgb(151, 134, 38)
:END:
#### Overview
##### [[Mask R-CNN]] plus an association head and two 3D convolutional layers.
##### [[https://i.imgur.com/b0RZUlH.png][TrackR-CNN Overview]]
#### Integrating temporal context
:PROPERTIES:
:heading: true
:END:
##### 3D convolutions (3rd dim is ^^time^^)
##### [[ResNet 101]] backbone
##### Augment features with temporal context
###### The output augmented features are then fed into [[region proposal network]]
###### [[LSTM]] can also be an alternative
#### Association Head
:PROPERTIES:
:heading: true
:END:
##### extend [[Mask R-CNN]] by an association head ([[fully connected layer]])
###### Input: [[region proposal network]]
###### output: predicts an **association vector** for each proposal
##### Association training
###### distance $d(v,w)$ between two association vectors $v$ and $w$ as the [[Euclidean distance]]
####### ^^(7)^^   $d(v,w):=||v-w||$
###### ^^Batch hard triplet loss^^ (for video sequences) [[Triplet loss]]
####### Each detection $d\in{\mathcal{D}}$ consists of a mask $\text{mask}_d$ and an association vector $a_d$
######## from time frame $t_d$
######## from ground truth track id $\text{id}_d$ determined by the overlap with the ground truth objects.
####### For a video of $T$ time steps, the association loss with margin $\alpha$:
######## ^^(8)^^
$$\frac{1}{|D|}\sum\limits_{d\in{\mathcal{D}}}\max\left(\max\limits_{  e\in{\mathcal{D}: \text{id}_e=\text{id}_d} }||a_e-a_d|| - \min\limits_{  e\in{\mathcal{D}: \text{id}_e\neq\text{id}_d} }||a_e-a_d|| + \alpha, 0\right)$$
#### Mask Propagation
:PROPERTIES:
:heading: true
:END:
##### Mask-based IoU together with optical flow warping is a strong cue for associating pixel masks.
##### For detection $d\in{\mathcal{D}}$ at time $t-1$ with mask $\text{mask}_d$ and a detection $e\in{\mathcal{D}}$ at time $t$ with mask $\text{mask}_e$
###### define mask propagation score as
######
$$\text{maskprop}(mask_d,mask_e)=\text{IoU}(\mathcal{W}(mask_d),mask_e)$$
###### where $\mathcal{W}(m)$ denotes warping mask $m$ forward by the optical flow between $t-1$ and $t$.
#### Tracking
:PROPERTIES:
:heading: true
:END:
##### For each class and frame $t$, we link together detections at the current frame that have detector confidence larger than a threshold $\gamma$ with detections selected in the previous frames
###### using the association vector distances (eqn (7))
##### We only choose the most recent detection for tracks from up to a threshold of $\beta$ frames in the past.
#####
