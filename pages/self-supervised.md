---
title: Self-supervised
---

## [[https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_12_12_Screenshot%202020-12-12%20141558.png?Expires=4761353769&Signature=j7W-Oq1ZDlVfsAKjhSiWAAi-IbKzhbFvjtusYr9rEbmZcz8sr8Y9D5jlFOdWETuWsec8q~ITBoma-ImLowLa8NHxZHq4oA0guvJif~gBDHaVIyJBxAKTO2ssoeVAbhL3IiLoaNIlrMBd3VoxEl7k-u~blceJ3-wS7~guan0JxSP6lQK5WxevvKWL~O3T4iLoOBelSh9Taj~SOpPbkZVsxFJ3HwFnldFb2mK8mDOC9czFDdFzJaTeVsdFv48YHEJQTdw9dPRTNLGYy-~OXDxvFZvs-1zdnhBaInltlxsil7bjVkdhIsbsK-MOcvDa9J7stFxj~1R8qD3zsaZ-v-NmzQ__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA][2020_12_12_Screenshot 2020-12-12 141558.png]] 
## 自监督学习能避免注释大型数据集带来的成本，即采用自定义pseudo-labels作为监督，并将学习的表示形式用于多个下游任务
### 具体而言，contrastive learning最近已成为在计算机视觉、自然语言处理（NLP）和其他领域中自监督学习方法的主要部分
### 推进原图像和其增强positive更近，而推开原图像和其negative更远
## 自监督学习方法集成了[[generative]]方法和[[contrastive]]方法，利用未标数据来学习基础表示
### pseudo-labels是一个普遍技术，帮助在各种pretext任务中学习特征
### 目前已经看到，在image-inpainting, colorizing greyscale images, jigsaw puzzles, super-resolution, video frame prediction, audio-visual correspondence等任务中，学习好的表示方式已经很有效
