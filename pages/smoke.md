---
title: smoke
---

## Keywords
:PROPERTIES:
:heading: true
:END:
### #[[3D Object Detection]] [[single-stage]] [[monocular]]
## Abstract
:PROPERTIES:
:heading: true
:END:
### Traditional 3D object detection -> multi-stage network
#### depends on region-based CNN ([[R-CNN]]) or [[region proposal network]]
#### Need to attach an additional network branch to
##### learn 3D information
##### or generate a psudo point cloud and feed it into point-cloud-detection network
#### brings in persistent noise from 2D detection
### Propose single stage network
#### learn 3D information directly from the image plane
## Abstract
## Structure
### ![image.png](/assets/pages_smoke_1610504667580_0.png)
###
