---
title: my contrib
---

## Unify global and local image features into one auto-encoder model to conduct image retrieval and keypoint extraction tasks simultaneously.
### Motivation
#### With the recent advances in robotics navigation and visual SLAM, loop closure has been studied substantially. To address it in practice, both image retrieval and geometric verification tasks need to be accomplished.
#### Therefore, keypoint extraction and description are required not only for local adjacent image frames as structure from motion but also for the system to validate whether the retrieved image pairs share the same landmark overlap via epipolar geometry.
#### However, traditional SLAM systems take local visual odometry and loop closure as separate stages, thus
### Deep learning architecture
## A versatile localization system with online and offline mode and multiple sensor support.
### The system builds a multi-view stereo (MVS) pipeline to obtain the trajectory and reconstruction of image collections.
### In addition to online visual-inertial SLAM system, our system can extensively optimize the whole image collection in offline mode to get an accurate prior map.
### With the prior map of arbitrary size the system is able to detect and retrieve revisited scenes to leverage localization accuracy.
### The incremental loop closing mechanism can largely reduce the searching time by checking consecutive loops detected near the current frame.
## Dynamic object-aware SLAM algorithm
### The system can estimate the camera poses along with static and dynamic environment, by extracting velocity information.
### The system exploits semantic information to enable motion estimation of rigid objects without any prior knowledge of the object shape.
### The system uses [[YOLO]] for instance detection.
