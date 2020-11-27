---
title: UPerNet
public: true
---

## Meta Data:
### #title Unified Perceptual Parsing for Scene Understanding, 2018
### #topic #Segmentation [[PSPNet]] [[FPN]] 

## [[mmsegmentation config]]
### model: upernet_r50

### default_runtime

### schedule: 80k, 160k

## Motivation: 
### Since scene recognition, object detection, texture and material recognition are intertwined in human visual perception, this raises an important question for the computer vision systems: is it possible for a neural network to solve several visual recognition tasks simultaneously? This motives our work to introduce a new task called [Unified Perceptual Parsing (UPP](Unified-Perceptual-Parsing-(UPP.md)) along with a novel learning method to address it.

## Challenges:
### 1. There is no single image dataset annotated with all levels of visual information. Various image datasets are constructed only for a specific task, such as [[ADE20K]]for scene parsing, the [[Describe Texture Dataset]] (DTD) for texture recognition, and OpenSurfaces for material and surface recognition.

### 2. Annotations from different perceptual levels are heterogeneous. For example, [[ADE20K]] has pixel-wise annotations while the annotations for textures in the DTD are image-level.

## Solution:
### 1. At each iteration, we randomly sample a data source, and ^^only update the related layers^^ on the path to infer the concepts from the selected source. Such a design avoids erratic behavior that the gradient with respect to annotations of a certain concept may be noisy. 

### 2. Our framework exploits the hierarchical nature of features from a single network, i.e., for concepts with higher-level semantics such as scene classification, the classifier is built on the feature map with the higher semantics only; for lower-level semantics such as object and material segmentation, classifiers are built on feature maps fused across all stages or the feature map with low-level semantics only.

## 1. Unified Perceptual Parsing (UPP)
### Recognition of many visual concepts as possible from a given image.

### Possible visual concepts are: scene labels, objects and object parts; materials and textures of objects.

### Construct an image dataset by combining several sources of image annotations.
#### Multi-source dataset enforce balancing

### Metrics 
#### Pixel Accuracy (P.A.):: proportion of correctly classified pixels

#### IoU (mIoU):: intersection-over-union between predicted and ground truth pixels, averaged over all object classes.

#### To balance samples across different labels in different categories we first randomly sample 10% of original images as the validation set. 

#### Then randomly choose an image both from the training and validation set, and check if the annotations in pixel-level are more balanced towards 10% after swapping these two images. The process is performed iteratively.

## 2. UPP Network
### UPerNet:: Unified Perceptual Parsing Network, based on [[FPN]](FPN.md). Apply a [[Pyramid Pooling Module (PPM)]] from [[PSPNet]] on the last layer of the backbone network before feeding to the top-down branch in [[FPN]]. ^^The aim is to unify parsing of visual attributes at multiple levels.^^

### FPN:: A generic feature extractor which exploits multi-level feature representations in an inherent and pyramidal hierarchy. It uses a top-down architecture with lateral connections to fuse high-level semantic information into middle and low levels with marginal extra cost. #definition
#### Issue: empirical [[Recpetive Field]] of CNN is small.  

### FCN:: Fully convolutional networks. To enable high-resolution predictions, [[Dilated Net]] is adopted. #definition

### Dilated Net:: Removes the stride of convolutional layers and adds holes between each location of convolution filters. To ease the side effect of down-sampling while maintaining the expansion rate for receptive fields. It is a de facto paradigm for semantic segmentation. #definition
#### Issues:
##### Prior works show that lower layers tend to capture local features such as corners or edge/color conjunctions, while higher layers tend to capture more complex patterns such as parts of some objects. So using the features with the high-level semantics is unfit to segment perceptual attributes at multiple levels, especially the low-level ones.

##### With deep layers like [[ResNet 101]], in a dilated segmentation framework, dilated convolution needs to be applied to both blocks to ensure that the max down-sampling rate of all feature maps does not exceed 8. But due to the feature maps within the two blocks are ^^increased to 4 or 16 times^^ of their designated sizes, both the computation complexity and GPU memory footprint are dramatically increased.  

### Network architecture
#### [[ResNet 101]] $$ \{C_2,C_3,C_4,C_5\} $$ and [[FPN]] $$ \{P_2,P_3,P_4,P_5\} $$ where $$ P_5 $$ follows [[Pyramid Pooling Module (PPM)]].  

#### Scene label:: the highest level attribute annotated at image-level.
##### Predicted by a [[Global Average Pooling]] of $$ P_5 $$ followed by a linear classifier.  

##### Unlike [[Dilated Net]], the down-sampling rate of $$ P_5 $$ is large so the features after [[Global Average Pooling]] focus more on high-level semantics.

#### Object label:: fusing all feature maps of [[FPN]] is better than only using the highest resolution ($$ P_2 $$).

#### Materials: On top of $$P_2$$ rather than fused features.

#### Texture label:: given at image-level, is based on non-natural images.

### ![https://remnote-user-data.s3.amazonaws.com/tTVklhTOhsJ_HMBdrVcdaw8KCn59cMDzRySYaUIYZpWwMfyvNrhPbMzrLo8uWg-Sbir72RiPq0bQ-ISHhJihozh2wImbuMkTeIt9Jp3_izdE1ILmElkJGww9PCodVcXB](https://remnote-user-data.s3.amazonaws.com/tTVklhTOhsJ_HMBdrVcdaw8KCn59cMDzRySYaUIYZpWwMfyvNrhPbMzrLo8uWg-Sbir72RiPq0bQ-ISHhJihozh2wImbuMkTeIt9Jp3_izdE1ILmElkJGww9PCodVcXB)

### UPerNet framework for Unified Perceptual Parsing.
#### ^^Top-left: ^^The Feature Pyramid Network (FPN) with a PPM appended on the last layer of the back-bone network before feeding to the top-down branch in FPN.

#### ^^Top-right: ^^We use features at various semantic levels. Scene head is attached on the feature map directly after the PPM since image-level information is more suitable for scene classification.

#### ^^Bottom: ^^The illustrations of different heads.