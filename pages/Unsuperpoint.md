---
title: Unsuperpoint
---

## Meta Data
:PROPERTIES:
:heading: true
:END:
### #title UnsuperPoint: End-to-end Unsupervised Interest Point Detector and Descriptor, 2019
### #topic  #Superpoint #Keypoint #[[Descriptor Extraction]] #Unsupervised-learning #[[Siamese Network]] #[[Self-supervised]]

### #code https://github.com/791136190/UnsuperPoint_PyTorch 

## **Abstract**: 
### Inspired by [[Superpoint]] without pseudo-gt generated from [[Synthetic Image Pairs]] but [[Siamese Learning]] ([[Self-supervised]]). Interesting/salient points emerge naturally after training.

### 相同点
#### 多任务网络同时估计关键点和描述子

#### 巧妙地利用 homography 建立对应关系，并用于training过程中

### 不同点
#### Requires no pseudo ground truth points, no SfM generated representations and the model is learned from only one round of training.

#### SuperPoint 把关键点检测表达成一个分类问题，对于一个 $$8\times 8$$的patch，前64维对应每个点是否为关键点的概率，最后一维对应没有关键点。This paper 把这个问题转化为 regression，detection head 输出的是每个patch中的关键点相对于基准坐标的 offset 比例

#### 同时对于每个 patch 是否存在关键点的置信度，这篇 paper 的处理方法更和 [[R2D2]] 更加类似，即不对位置预测做限制，增加一个 score head 去评价每个 patch

#### training loss 上限制了keypoint在patch中的位置分布，略微有一点tricky.  This paper has tons of losses! Balancing them is quite a task.	

### Using a self-supervised approach, we use [[Siamese Network]] and a novel loss function that enables interest point scores and positions to be learned automatically.

## 1. Network Architecture
:PROPERTIES:
:heading: true
:END:
### Multi-task training with a shared backbone followed by multiple task-specific sub-modules.

### Backbone provides a downsampled feature map.

### Submodules are designed to produce an aligned output where each entry represents a point $$ m $$ with a position $$ \mathbf{p}_m $$, score $$ \mathbf{s}_m $$ and descriptor $$ \mathbf{f}_m $$.
### [[https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_11_26_unsuper.png?Expires=4759984377&Signature=gJcw1ErBjg9XqdkWPzESTVSn4UFUAFbryL1a5JM1Zqh52t3ABXt39hJYRbzym9rjMfsOauvRWjVGfxiKLbZDiSwjEvPpDM5-oWCHzIWaHhzZdgG9dy-hgc8Qfr9qTzqoM4D1bemLlM47TPL~nEPSttcZIKN2uv-LrhdgFAOXIAKxoIxM8HFed52M5dzkJ7DS66SmgtN3EwCcyjqOKFgQqhEHrzhMAwlomPuvtYfMoyuXGPzWmPkFaF7J0s2IlHWu9Gd4eMJ3prPdbLiTFe206cFQ0BwITHK9Jjm0YckybzJeYlzB7DdSGxHOWzXMr3sqiTfiWOUhH6Dqm2CMMu5eEg__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA][2020_11_26_unsuper.png]]
### ![](https://remnote-user-data.s3.amazonaws.com/cYHNsMuPSGHWFQFrx2zVsMtmG9K0YkqPC2TNl-tUIZcZQ6sWU_tpbeV7m8azJkfN0IzfXnEMsMnx4PC2Ew2gazZ-mfZHFDkwHtal2Hr9eRF6xt_O919nyNoG2rJgcQ6S)
## **1.1 Overview and notation**  	
### Each point position expressed by its relative position $$ \mathbf{P}_{relative} $$ and can be transformed to image pixel coordinate $$ \mathbf{P}_{map} $$.		

### Score $$ \mathbf{S}_{map} $$ is the ^^fitness^^ of each point and used for sampling the ^^best^^ $$ N $$ points. 	

### Descriptor $$ \mathbf{F}_{map} $$ has embedding of $$ F $$ channels for each entry (uniquely match correspondeces)	

### They are reshaped and sorted by highest score into respectively a vector $$ \mathbf{s} $$ with $$ M $$ elements, an $$ M\times 2 $$ matrix $$ \mathbf{P} $$ and an $$ M\times F $$ matrix $$ \mathbf{F} $$, where $$ M=\frac{H}{8}\cdot\frac{W}{8} $$ represents all predicted points.

### All conv layers have stride 1 and kernel size of 3, followed by [[Batch Normalization]] and [[Leaky ReLU]].  

## **1.2 Score module**  
### The score module regresses a score for each entry in the final feature map.

### Two conv layers with $$ 256 $$ and 1$$$$channels.

### Sigmoid activation to bound in interval $$ [0,1] $$

## **1.3 Position module**  
### Predicts a relative image coordinate for each output entry and maps it to an image pixel coordinate.

### Two conv layers with $$256 $$ and $$2$$ channels. 

### **Sigmoid activation** to bound in interval $$ [0,1] $$  

### For a network with 3 poolings layers (a subsampling factor of 8), a relative position is predicted for each $$ 8\times 8 $$ region in the input image.			
#### For [[Superpoint]] and [[Lf-net]], top interest point locations are selected from a heat map of the ^^same size ^^as an input image.

#### Regression is differentiable and enables fully unsupervised training.

#### Only predicting a single point for each $$ 8\times 8 $$ area adds functionality similar to [[NMS]]. On top of [[NMS]], it removes closely clustered points and interest points are more ^^homogeneously distributed^^.  

### For small input of image $$ 24\times 24 $$:![](https://remnote-user-data.s3.amazonaws.com/s2JMQz5x4BbtZgDszuKM32ePBhWfm5X-N297zDen_5XNrTll-b8OZ5AaT13AnQA25VcQpARCP5YHpa4FIDO68I0HvsTQNYJu_vmpEKIEJhmkofh6xJ73-v9z1lTVR4wm)  
			

### Then $$ \mathbf{P}_{relative} $$ is transformed to image pixel coordinate $$ \mathbf{P}_{map} $$ by multiplying with downsampling factor $$ f_{down}=8 $$.  
		

## **1.4 Descriptor module**  
### Generates a descriptor for each entry.	

### Two conv layers with $$ 256 $$ and $$ F=256 $$$$$$channels, final layer without activation.

### Interpolation of descriptors is integrated into the model. The model use all point positions in $$ \mathbf{P}_{map} $$ to interpolate all entries in descriptor map $$ \mathbf{F}_{map} $$. 

### In [[Superpoint]] the interpolation is a post-processing step used under inference.  

## **2. Self-supervised Framework**
### Three tasks are leaned simultaneously, in a siamese network to predict interest points for two augmentations of the same input image
	![](https://remnote-user-data.s3.amazonaws.com/usy375CivfxdYkqm-lhodts5Cl3aoAs_ZSRIj823vV_Ujyt9s_nZP4QAN4OP4HjD9I4ig7ntlGxEDBSD0-n8H_9DlXfuqRH8HKj49VdOk0YwVoWxO71liFpWe5rBmqan)
	

### The two augmentations and their predictions are separated into individual branches.
#### Branch A (blue) is a non-wrapped version of the input image

#### Whereas branch B (red) is a wrapped version of the input image.
##### The image in branch B is spatially transformed by a random homography $$ T $$ (^^rotation, scale, skew and perspective transforms^^). 

### The image for each branch is transformed by independent random non-spatial image augmentations such as ^^brightness and noise^^.

### Point psitions of branch A are transformed by $$ T $$ to spatially align points from branch A to branch B. If they are spatially close after alignment, we define points to correspond.

## **3. Loss functions**  
### The total loss $$ \mathcal{L}_{total} $$ consists of 4 loss terms

### (1)     $$ \mathcal{L}_{total} = \alpha_{usp}\mathcal{L}^{usp}+\alpha_{uni\_xy}\mathcal{L}^{uni\_xy}+\alpha_{desc}\mathcal{L}^{desc}+\alpha_{decorr}\mathcal{L}^{decorr} $$
#### $$ \mathcal{L}^{usp} $$ is UnSupervised Point ([[USP]]) loss to learn position and score of interest points.

#### $$ \mathcal{L}^{uni\_xy} $$ is regularization term to encourage a uniform distribution of relative point positions.

#### $$ \mathcal{L}^{desc} $$ is descriptor loss  and $$\mathcal{L}^{decoor}$$ is regularization to reduce overfitting by decorrelating descriptors.

### 3.1 [[Point-pair]] correspoindences
#### Each branch $$ b\in \{A, B\} $$ outputs three tensors $$ \mathbf{s}^b $$, $$ \mathbf{P}^b $$ and $$ \mathbf{F}^b $$.  We need to establish point correspondeces.

#### Determine an $$ M^A\times M^B $$ distance matrix $$ \mathbf{G} $$ by computing pairwise distances between all $$ M^A $$ transformed points from branch A and all $$ M^B $$ points from branch B.

#### (2)    $$ \mathbf{G}=[g_{ij}]_{M^A\times M^B}=\left[ || \mathbf{p}_i^{A\rightarrow B}-\mathbf{p}_j^B || _2\right]_{M^A\times M^B} $$  
where each entry $$ g_{ij} $$ in $$ \mathbf{G} $$ is the Euclidean distance between a transformed point $$ \mathbf{p}_i^{A\rightarrow B}=T\mathbf{p}_i^A $$ with index $$ i $$ in branch A and a point $$ \mathbf{p}^B_j $$ with index $$ j $$ in branch B.	

#### Not all points in branch A are merged into [[point-pair]]s (might not have a nearby neighbour in B)

#### Criterian of being point pair: the distance $$ g_{ij} $$ less than a ^^minimum distance^^^^$$\epsilon_{corr} $$^^^^.^^  

#### Redefine output tensors to the new set of tensors defined as ^^**corresponding tensors**^^^^ ^^with $$ K $$ entries so that each entry$$ k $$in the ^^**corresponding tensors**^^ maps to the same point in the input image. 
##### For each entry $$ k $$ in branch $$ b $$, [[point-pair]] score $$ \hat{s}^b_k $$, position $$ \hat{\mathbf{p}}_k^b $$ and descriptor $$ \hat{\mathbf{f}}_k^b $$.  	

##### Define distance $$ d_k $$ as
(3)    $$ d_k=||T\hat{\mathbf{p}}_k^A - \hat{\mathbf{p}}_k^B|| =$$$$||T\hat{\mathbf{p}}_k^{A\rightarrow B} - \hat{\mathbf{p}}_k^B||$$$$  $$ 
	

### **3.2 Unsupervised point loss **$$\mathcal{L}^{usp}$$$$  $$
#### The goal is to improve [[repeatability]] of the detector (regardless of camera viewpoint)

#### UnSupervised Point loss:: #definition
##### Uses [[point-pair]]s to train a  detector using an [[Unsupervised-learning]] loss function.	

##### Three terms and accumulated over all $$ K $$corresponding [[point-pair]]s.

##### (4)   $$ \mathcal{L}^{usp}=\alpha_{position}\sum\limits_{k=1}^K l_k^{position}+\alpha_{score}\sum\limits_{k=1}^K l_k^{score}+\sum\limits_{k=1}^K l_k^{usp}$$  	
###### $$ l_k^{position} $$ensures the predicted positions of a [[point-pair ]]represent the same in input image by minimizing the distance for each point-pair$$ k:  $$(5)   $$ l_k^{position}=d_k$$.

###### Initially a [[Siamese Network]] predicts random positions and gradually reduce distances.

###### $$ l_k^{score} $$ensures the predicted scores for a point-pair are similar by minimizing ^^squared distance^^ between score values for each point-pair $$ k $$: (6)   $$ l_k^{score}=(\hat{s}_k^A - \hat{s}_k^B)^2 $$.	

###### In image matching similar score for points (in multi-viewpoints) represents the same point.

###### $$ l_k^{usp} $$ ensures the predicted scores acutally represent the confidence of interest points. The highest score should be the most repeatable points: (7)   $$ l_k^{usp}=\hat{s}(d_k-\bar{d}) $$.

###### $$ \hat{s}_k $$denotes the joint score of a [[point-pair]]: (8) $$ \hat{s}_k=\frac{\hat{s}_k^A+\hat{s}_k^B}{2} $$  

###### $$ \bar{d} $$ denotes the average distance between all point-pairs: (9)    $$ \bar{d}=\frac{1}{K}\sum\limits_{k=1}^K d_k $$.

###### The core concept of $$ l_k^{usp} $$ is that the network should define a good interest point as a point with a low point-pair correspondence distance $$ d_k $$. Oppositely, for a bad interest point, $$ d_k $$ is large, because the network is unable to predict point positions consistently.

### **3.3 Uniform point predictions** $$ \mathcal{L}^{uni\_xy}$$
#### Optimally, the relative$$ x- $$and$$ y- $$coordinate predictions should be uniformly distributed within the $$ 8\times 8 $$area. But there are a large number of points near the boundaries.
##### A model is encouraged, especially for hardly repeatable points, to place the point as closely to points outside its own region in order to minimize $$ d_k $$.

#### So a new loss function is proposed to encourage a uniform distribution.	

#### Ascendingly sorted values sampled from a uniform distribution will approximate a straight line going from the lower to the upper bound within the specified range.

#### Define a heuristic measure $$ D(\mathcal{U}, \mathcal{V}) $$to calculate distance between a uniform distribution $$ \mathcal{U}(a,b) $$ and some distribution$$ \mathcal{V} $$in a bound interval$$ [a,b]$$. 	
##### $$ L $$ values are sampled from $$ \mathcal{V} $$ to form a vector $$ \mathbf{v}$$.   

#### The distance between distributions are:	
##### (10)     $$ D\left(\mathcal{U}(a,b), \mathcal{V}\right)=\sum\limits_{i=1}^{L}\left( \frac{v_i^{sorted}-a}{b-a}-]\frac{i-1}{L-1}\right)^2 $$  

##### $$ v_i^{sorted} $$ is the ascendingly sorted values of$$ \mathbf{v} $$such that$$ v_i^{sorted}<v_{i+1}^{sorted} $$for $$ i\in\{1,2,\cdots,L-1\} $$.

##### The first term normalizes sorted values to interval $$ [0,1] $$

##### The second term is a proportional line from 0 to 1.

#### ^^Pros: ^^Differentiable and does not require predictions to be discretized.

#### $$ \mathcal{L}^{uni\_xy} $$ is the distance between a uniform distribution and the distribution of relative image positions $$ \mathbf{P}_{relative,x} $$ and $$ \mathbf{P}_{relative,y} $$, individually.
##### This loss is calculdated independently for each branch, does not rely on point correspondences.

##### (11)  $$ \mathcal{L}^{uni\_xy}=\alpha_{uni\_xy}\left(\mathcal{L}^{uni\_x}+\mathcal{L}^{uni\_y}\right)$$  

##### (12)  $$ \mathcal{L}^{uni\_x}=\sum\limits_{i=1}^M \left(x_i^{sorted}-\frac{i-1}{M-1}  \right)$$  

##### (13)  $$\mathcal{L}^{uni\_y}=\sum\limits_{i=1}^M \left(y_i^{sorted}-\frac{i-1}{M-1}  \right)$$ 

#### ![](https://remnote-user-data.s3.amazonaws.com/j387aejnMy-WMa9_fcumqPoXLGBTY017wHeC7YFMbdaO3UKoUxOISfJTtSQ7uOk_z6rlBFv3IX630xDo-5m3zBBsjnboDfmGFLFK8o_uosClkSbBN0248l8mgua0rTEE)  
	

### **3.4 Descriptor** $$ \mathcal{L}^{desc}$$
#### Determined using a [Hinge Loss](link_generated_on_download) with a positive and negative margin as described in [Superpoint](link_generated_on_download).

#### We define an $$ M^A\times M^B $$correspondence matrix $$ \mathbf{C} $$ containing values of either $$ 0 $$ or $$ 1$$ .
##### Each entry $$ c_{ij} $$ specifies if two points are separated by ^^less than 8 pixel^^  for any pair combination of transformed points in branch A, $$ \mathbf{p}_i^{A\rightarrow B} $$ where $$ i\in \{1,2,\cdots,M^A\} $$, and points in branch B, $$ \mathbf{p}^{B}_j $$ where $$ j\in\{1,2,\cdots,M^B\} $$.
###### Unlike point-pairs, a single point may correspond to multiple points in other branch

###### (14)   $$ c_{ij}=\begin{cases} 1 \; & \; \text{if} \; g_{ij}\leq{8} \\ 0 \; & \; otherwise \end{cases}$$  

#### Hinge loss (positive margin $$ m_p $$ and negative margin $$ m_n $$)	
##### (15)    $$ \mathcal{L}^{desc} = \sum\limits_{i=1}^{M^A}\sum\limits_{j=1}^{M^B} l_{ij}^{desc} $$  $$$$  
$$ \begin{aligned} l_{ij}^{desc} & = \lambda_d \cdot c_{ij} \cdot \max\left(0,m_p - {\mathbf{f}_i^A}^\top\mathbf{f}_j^B\right) \\ 
& + (1-c_{ij})\cdot \max\left( 0, {\mathbf{f}_i^A}^\top\mathbf{f}_j^B-m_n \right) 
\end{aligned}$$$$ $$  
	

### **3.5 Decorrelate descriptor** $$ \mathcal{L}^{decorr}$$  
#### Feature descriptors are decorrelated to reduce ^^overfitting^^ and improve compactness.

#### Similar to [[L2-Net]], we reduce the correlation between dimensions by minimizing the off-diagonal entries of the descriptor correlation matrix $$ \mathbf{R}^b=\left[r_{ij}^b\right]_{F\times F} $$for each branch$$ b $$.
##### (16)     $$ \mathcal{L}^{decorr}=\sum\limits_{i\neq {j}}^F \left(r_{ij}^A\right)+\sum\limits_{i\neq{j}}^F\left(r_{ij}^B\right) $$  

##### Each entry $$ r_{ij}^b $$ in $$ \mathbf{R}^b $$ is$$$$

##### (17)  $$ r_{ij}^b=\frac{(\mathbf{v}_j^b-\bar{v}_j^b)^\top(\mathbf{v}_i^b-\bar{v}_j^b)}{\sqrt{(\mathbf{v}_j^b-\bar{v}_j^b)^\top(\mathbf{v}_i^b-\bar{v}_j^b)}\sqrt{(\mathbf{v}_j^b-\bar{v}_j^b)^\top(\mathbf{v}_i^b-\bar{v}_j^b)}} $$,
where $$ \mathbf{v}_i^b $$is a $$ M^b\times 1 $$ sized vector and is the $$ i\text{th} $$ column of $$ \mathbf{F}^b$$,
$$ \bar{v}_i^b $$ is the mean of $$ \mathbf{v}_i^b $$.  

## **4. Experiments**
### **4.1 Metrics**	
#### Repeatability Score:: #definition
##### Measures the quality of interest points and is the ratio between the number of points observed by both viewpoints and the total number of points.	

##### For a planar scene, the point correspondences between two camera views can be established by simply mapping points from one view to the other using a homography. 

##### To account for localization errors between two corresponding points, we define points to correspond if they are below a certain pixel distance defined as the correct distance$$ \rho $$.

##### To only evaluate points that are observable in both views, the repeatability measure will only include points in a region shared by the two viewpoints. 

##### The repeatability is therefore the average re- peatability calculated in the view of each camera.

#### Localization Error:: #definition
##### The average pixel distance between corresponding points. (LE)			

##### Only point-pairs with distances below $$ \rho $$are included in the calculation. Like [Repeatability Score](link_generated_on_download), the localization error is the average error of corresponding points calculated in both camera views.

#### **Homography Estimation Procedure**		
##### ![](https://remnote-user-data.s3.amazonaws.com/6JNFpcozQYjcUzhqPkSvxxHU9vpWx_qY6sCVge3qRG1BNfEyOF22g6JmCrVm4r6NFzDa-dyQQQ6zqmB5tUnd6bynWcj08WH0_A4mUl3BeO2OknhkEkEBvUKTHUAnh0VA)

#### **Matching score**
##### [[M-score]]:: Matching score. The average ratio between GT correspondences recovered and total number of estimated features within the shared viewpoint region when matching back and forth.
