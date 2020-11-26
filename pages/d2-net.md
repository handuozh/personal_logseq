---
title: D2-Net
public: true
published: true
permalink: d2-net
---

## Meta Data
### [dusmanuD2NetTrainableCNN2019]([[D2-Net: A Trainable CNN for Joint Description and Detection of Local Features]])

### [[Dusmanu, Mihai]], [[Rocco, Ignacio]], [[Pajdla, Tomas]], [[Pollefeys, Marc]], [[Sivic, Josef]], [[Torii, Akihiko]], & [[Sattler, Torsten]] #authors

### [[2019]] #issued

### #title D2-Net: A Trainable CNN for Joint Description and Detection of Local Features, 2019, CVPR
### #[[topic]]: #Keypoint #[[Keypoint Extraction]]  #[[Descriptor Matching]]  #Detection-and-describe

## [[Detect-then-describe]]:: Classical interest point detection and description method where separated handcrafted methods are used to first identify repeatable keypoints and then represent them with a local descriptor. #definition

## [[Detection-and-describe]]  paradigm
### Compare two paradigms
[[https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_11_25_compair_two_paradigms.png?Expires=4759878442&Signature=PiQEPrEH67zjnYttENAuUb73AMKxr5T7y2T8b8taVom1xm85nxQ76L3qGpW1an6YWcMzZIUaOcIjgOW0DTbSuTTnm-UxmM0Sif2tC7XQ4K8NzZz3ZbxbTBFv6J22knQanPOt3P96h9tRlVqLfeRZrQiWnm~l03FFftV9pavgu5kx7BC8MYPS-woBXYccqdBI4jkBaF4RfOS6igUGSWbN0QWw4Kk5zCxhHa1rMYoqeS3N8bSJZBQeUMy54MvSoknSIRPOfwll4e7FFYeoi~a900U7PHVRcIEzJ5MDG~Fc38K2Y7k8gawqNTDdVfCR0sNmp9tLJpuGUoWjiP1l0pXWOg__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA][2020_11_25_compair_two_paradigms.png]]
## In the paper both detector and descriptor share the underlying representation, as shown below: 
[[https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_11_25_overview.png?Expires=4759878457&Signature=fiQhdlrka5Yjavx0YayzUK9iMTHZ1T-nb1mCoiMRpAOhQ4ur2VxHWDTC0LTHEgbHAE2K8mk8k7U8Tn~E0kZRUfuB5H54OJ9O~r1k8UryIHjCkl-GoLQLHfpCF5AJ69prPUgIx3uCdgvgrIS8hCvNZJEhQ5sI56rIjIXP35a8AGm9Ts0xgOF-jZ0pJRPEW-X8HzyvRgohqQYSNlDuIoZrkgj0bt0p8F4fSuT~uqAWydJip76fCaXchoZAxHXbK8lbq1HdnXXFbPyG~jGUrS4SiS8WMz4g-t35SMGXuDl6veFi53xn5addiLNfYZX32YIjm0UbJXZ6v4cUy-eqjkBZKQ__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA][2020_11_25_overview.png]]
## 1. Feature Description
### Interpretatioin of feature 3D tensor $F$ is a dense set $\mathbf{d}$:
#### (1)    $$ \mathbf{d}_{ij} = F_{ij}, \mathbf{d} \in {\mathbb{R}^n}$$
where $n$ is channel size, $i=1,\cdots, h$, $j=1,\cdots,w$
### Compare using [[L2-norm]]

## 2. Feature Detection
### Collection of 2D responses $D$ from 3D tensor $F$ :
#### (2)   $$D^k=F_{: :k},   \; D^k \in{\mathbb{R}^{h\times w}}$$

#### where $n$ is channel size, $k==1,\cdots,n$
### 2.1 Hard Feature Detection
#### For a point $(i,j)$ to be detected, we need:
##### (3)      $(i,j) is a detection $\Longleftrightarrow D_{ij}^k$ is a local max in $D^k$
 with $k=\argmax\limits_t D_{ij}^t$
### 2.2 Soft Feature Detection
#### Amenable for back propagation.

#### Define a soft local-max score
##### (4)            $$ \alpha_{ij}^k=\exp(D_{ij}^k)/\sum_{(i^{\prime},j^{\prime})\in\mathcal{N}(i,j)} \exp(D_{i'j'}^k) $$
where $$ \mathcal{N}(i,j) $$ is the set of 9 neighbours of pixel $$ (i,j) $$.

#### Define a soft channel selection to compute a [Ratio-to-max](Ratio-to-max.md) per descriptor that emulates channel-wise non-maximum suppression:
##### (5)            $$ \beta_{ij}^k = D_{ij}^k/\max\limits_t D_{ij}^t $$  

#### Together we maximize the product of both scores across all feature maps $$ k $$ to obtain single score map:
##### (6)            $$ \gamma_{ij}=\max\limits_k (\alpha_{ij}^k \beta_{ij}^k $$)

#### Image-level normalization:
##### (7)             $$ s_{ij}=\gamma_{ij} / \sum\limits_{(i',j')} \gamma_{i'j'} $$

### 2.3 Multiscale Detection

## 3. Jointly Optimizing detection and description