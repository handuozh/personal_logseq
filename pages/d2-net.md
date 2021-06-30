---
title: D2-Net
type: Article
publish date: [[6/2019]]
proceeding: [[CVPR]]
citekey: dusmanuD2NetTrainableCNN2019
tags: [[Keypoint-Extraction-and-Description]] , [[Detection-and-describe]] , [[Keypoint]] , #zotero, #literature-notes, #reference
authors: [[Mihai Dusmanu]], [[Ignacio Rocco]], [[Tomas Pajdla]], [[Marc Pollefeys]], [[Josef Sivic]], [[Akihiko Torii]], [[Torsten Sattler]]
tags: #Keypoint, [[Keypoint Extraction]],  [[Descriptor-Matching]],  #Detection-and-describe
public: true
---

- Meta Data
  heading:: true
  	- #title D2-Net: A Trainable CNN for Joint Description and Detection of Local Features, 2019,s CVPR
  	- #url: [https://ieeexplore.ieee.org/document/8953622/](https://ieeexplore.ieee.org/document/8953622/)
	- PDF Attachments
	- [Dusmanu et al. - 2019 - D2-Net A Trainable CNN for Joint Description and .pdf](zotero://open-pdf/library/items/WIIQKKCT)
	- [2019_Dusmanu et al_2019 IEEECVF Conference on Computer Vision and Pattern Recognition (CVPR)_D2-Net.pdf](zotero://open-pdf/library/items/G3S427UG)
	- zotero items: [Local library](zotero://select/items/1_SGT9GB2D)
	- [[abstract]]:
		- We propose an approach where a single convolutional neural network plays a dual role:
			- 同时提取detector 和 descriptor
			- By **postponing the detection to a later stage**, the obtained keypoints are more stable than their traditional counterparts based on early detection of low-level structures.
			- We show that this model can be trained using pixel correspondences extracted from readily available large-scale **SfM** reconstructions, without any further annotations.
				- 通过SfM的数据集中的gt获取transform的ground truth of pixel correspondence 信息
		- The proposed method obtains state-of-the-art performance on both the difﬁcult [[Aachen]] Day-Night localization dataset and the **InLoc** indoor localization benchmark.
		- 从两个角度获得特征点
			- depth-wise max
			- local spatial max
			  	- #related
		- 缺点
			- {{embed ((605c4b6f-abef-4c73-a5e0-c31197183ad0))}}
			- 作为localization 精确度不够
- [[Detect-then-describe]]: Classical interest point detection and description method where separated handcrafted methods are used to first identify repeatable keypoints and then represent them with a local descriptor. #definition
  id:: 5fffb4b3-72ca-47f6-b439-7d63456a4aff
- [[Detection-and-describe]]  paradigm
	- Compare two paradigms
	  [[https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_11_25_compair_two_paradigms.png?Expires=4759878442&Signature=PiQEPrEH67zjnYttENAuUb73AMKxr5T7y2T8b8taVom1xm85nxQ76L3qGpW1an6YWcMzZIUaOcIjgOW0DTbSuTTnm-UxmM0Sif2tC7XQ4K8NzZz3ZbxbTBFv6J22knQanPOt3P96h9tRlVqLfeRZrQiWnm~l03FFftV9pavgu5kx7BC8MYPS-woBXYccqdBI4jkBaF4RfOS6igUGSWbN0QWw4Kk5zCxhHa1rMYoqeS3N8bSJZBQeUMy54MvSoknSIRPOfwll4e7FFYeoi~a900U7PHVRcIEzJ5MDG~Fc38K2Y7k8gawqNTDdVfCR0sNmp9tLJpuGUoWjiP1l0pXWOg__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA][2020_11_25_compair_two_paradigms.png]]
- In the paper both detector and descriptor share the underlying representation, as shown below: 
  [[https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_11_25_overview.png?Expires=4759878457&Signature=fiQhdlrka5Yjavx0YayzUK9iMTHZ1T-nb1mCoiMRpAOhQ4ur2VxHWDTC0LTHEgbHAE2K8mk8k7U8Tn~E0kZRUfuB5H54OJ9O~r1k8UryIHjCkl-GoLQLHfpCF5AJ69prPUgIx3uCdgvgrIS8hCvNZJEhQ5sI56rIjIXP35a8AGm9Ts0xgOF-jZ0pJRPEW-X8HzyvRgohqQYSNlDuIoZrkgj0bt0p8F4fSuT~uqAWydJip76fCaXchoZAxHXbK8lbq1HdnXXFbPyG~jGUrS4SiS8WMz4g-t35SMGXuDl6veFi53xn5addiLNfYZX32YIjm0UbJXZ6v4cUy-eqjkBZKQ__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA][2020_11_25_overview.png]]
- 1. Feature Description
  heading:: true
	- Interpretatioin of feature 3D tensor $F$ is a dense set $\mathbf{d}$:
		- (1)    $$ \mathbf{d}_{ij} = F_{ij}, \mathbf{d} \in {\mathbb{R}^n}$$
		  where $n$ is channel size, $i=1,\cdots, h$, $j=1,\cdots,w$
	- Compare using [[L2-norm]]
- 2. Feature Detection
  heading:: true
	- Collection of 2D responses $D$ from 3D tensor $F$ :
		- (2)   $$D^k=F_{: :k},   \; D^k \in{\mathbb{R}^{h\times w}}$$
		- where $n$ is channel size, $k==1,\cdots,n$
	- **Hard** Feature Detection
		- For a point $(i,j)$ to be detected, we need:
			- (3)      $(i,j)$ is a detection $\Longleftrightarrow D_{ij}^k$ is a local max in $D^k$
			   with $k=\argmax\limits_t D_{ij}^t$
		- **Soft** Feature Detection
		  id:: 605c055f-6bea-4393-9330-3dc44d17a069
			- Amenable for back propagation.
			- Define a soft local-max score
				- (4)            $$ \alpha_{ij}^k=\exp(D_{ij}^k)/\sum_{(i^{\prime},j^{\prime})\in\mathcal{N}(i,j)} \exp(D_{i'j'}^k) $$
					- where $$ \mathcal{N}(i,j) $$ is the set of 9 neighbours of pixel $$ (i,j) $$.
			- Define a soft channel selection to compute a [Ratio-to-max](Ratio-to-max.md) per descriptor that emulates channel-wise non-maximum suppression:
				- (5)            $$ \beta_{ij}^k = D_{ij}^k/\max\limits_t D_{ij}^t $$
			- Together we maximize the product of both scores across all feature maps $$ k $$ to obtain single score map:
				- (6)            $$ \gamma_{ij}=\max\limits_k (\alpha_{ij}^k \beta_{ij}^k $$)
			- Image-level [[normalization]] :
				- (7)             $$ s_{ij}=\gamma_{ij} / \sum\limits_{(i',j')} \gamma_{i'j'} $$
- 2.3 Multiscale Detection
- 3. Jointly Optimizing detection and description
  heading:: true