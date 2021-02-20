---
title: transformer
---

## [[sequence-to-sequence]] model by using [[self-attention]]

### The original paper: [[Attention is all you need]] , NIPS, 2017.
### Purely based [[attention]] and dense layers
### Higher accuracy than [[RNN]]s on large datasets
## Attention Layer in [[attention]] without [[RNN]]
:PROPERTIES:
:heading: true
:background_color: #793e3e
:END:
### Encoder's inputs are vectors $\mathbf{x}_1, \mathbf{x}_2, \cdots, \mathbf{x}_m$
### Decoder's inputs are vectors $\mathbf{x}_1^{\prime}, \mathbf{x}_2^{\prime}, \cdots, \mathbf{x}_t^{\prime}$
### **keys** and **values** are based on encoder's inputs
#### keys: $\mathbf{k}_{:i}=\mathbf{W}_K \mathbf{x}_i$
#### value: $\mathbf{v}_{:i}=\mathbf{W}_V \mathbf{x}_i$
#### [attention layer](https://i.imgur.com/lRuneZW.png)
### **queries** are based on decoder's inputs
#### Query: $\mathbf{q}_{:j}=\mathbf{W}_Q \mathbf{x}_j^{\prime}$
### Compute weights: $\alpha_{:1}=\rm{Softmax}(\mathbf{K}^{\top}\mathbf{q}_{:1})\in{\mathbb{R}^m}$.
#### [a1](https://i.imgur.com/MtGhesX.png)
### Compute context vector: $\mathbf{c}_{:1}=\alpha_{11} \mathbf{v}_{:1} + \cdots + \alpha_{m1} \mathbf{h}_{:m}=\mathbf{V\alpha}_{:1}$
#### [c1](https://i.imgur.com/vAiQ7AO.png)
### Compute weights: $\alpha_{:2}=\rm{Softmax}(\mathbf{K}^{\top}\mathbf{q}_{:2})\in{\mathbb{R}^m}$.
### Repeat until
#### Output of attention layer $\mathbf{C} = [\mathbf{c}_{:1}, \mathbf{c}_{:2}, \cdots, \mathbf{c}_{:t}]$.
#### $\mathbf{c}_{:j}=\mathbf{V} \cdot \rm{Softmax}\left(\mathbf{K}^{\top}\mathbf{q}_{:j}\right)$.
##### Thus $\mathbf{c}_{:j}$ is a function of $\mathbf{x}_j^{\prime}$ and $[\mathbf{x}_1, \cdots, \mathbf{x}_m]$.
### Attention layer: $\mathbf{C}=\rm{Attn}\left(\mathbf{X, X}^{\prime}\right)$.
#### $\mathbf{X}$ encoder's inputs
#### $\mathbf{X}^{\prime}$ decoder's inputs
#### Parameters $\mathbf{W}_Q, \mathbf{W}_K, \mathbf{W}_V$
#### [atten layer](https://i.imgur.com/5yTcLzG.png)
## Attention Layer in [[self-attention]] without [[RNN]]
:PROPERTIES:
:heading: true
:background_color: #49767b
:END:
### $\mathbf{C}=\rm{Attn}\left(\mathbf{X,X}\right)$.
#### Inputs: $\mathbf{X}=[\mathbf{x}_1, \mathbf{x}_2, \cdots, \mathbf{x}_m]$
#### Parameters $\mathbf{W}_Q, \mathbf{W}_K, \mathbf{W}_V$
#### [self-atten](https://i.imgur.com/x9iLP0L.png)
### Single-Head [[self-attention]]
#### As seen above which is called "single-head self-attention"
## Multi-Head [[self-attention]]
:PROPERTIES:
:heading: true
:id: 602f1494-1008-4ce6-8660-be856c3cfecd
:END:
### Using $l$ single-head self-attentions (^^Do not share parameters^^)
#### A single-head self-attention has 3 paramter matrices: $\mathbf{W}_Q, \mathbf{W}_K, \mathbf{W}_V$.
#### Totally $3l$ parameters matrices.
### Concatenating outputs of single-head ones
#### Single-head outputs are $d\times m$ matrices
#### Multi-head output shape: $(ld)\times m$.
## Self-Attention Layer + Dense Layer
:PROPERTIES:
:heading: true
:END:
## The [[Pointwise]] [[fully connected layer]] is called ^^feed-forward network^^.
:PROPERTIES:
:id: 602f1494-f1af-496b-88e8-0890899d2c25
:END:
### linear transformers
### non-linear [[activation function]]s
##
