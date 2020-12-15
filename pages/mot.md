---
title: MOT
---

## MOT based on Deep Features and Localization info
:PROPERTIES:
:heading: true
:END:
### Given information
#### object class
#### object bounding box coordinates
#### object features
### Dissimilarity score between objects in current frame and track history
### [[Hungarian]] to make decisions
### [[https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_12_14_Ch4Fig1.jpg?Expires=4761537078&Signature=PD1cVxzUQvQQtRUYI5JQOHxePq3HLeOQakpzbyxDnh6TuE8QbVntZaaee7FCNN4rGOwNlMxuJ7WyParwzmeIW-HorspzRhRszFxH73X2S8ipQIEcYej6yuScZqFisqh0yX51f4PngPPSmDOZs6ICiKCEHKcFZFDr4T751zCT7R1fhnlvSZyJACiXNzuJNczbLuFw~4TRgEGttZcFq6vG9SMdPglP01iX9ESn-BMsX-FAKCWdjYit8fr4SqznwRkXNJKgjwnxPYfi2uu18dLwYTjPPfJAVU4ecfIMaqQI6XcbMQmNTk-6vxUGZsaIp3-C1Gk1rdQchvx3-XLMIswT-w__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA][2020_12_14_Ch4Fig1.jpg]]
### Data Association
:PROPERTIES:
:heading: true
:END:
#### Dissimilarity cost for objects detected in current frame and tracking buffer.
#####
$$\text{dis}(i,j)=\left{ \begin{matrix} 1; & \text{if} i_{cls}\neq{j_{cls}} \\ 0.5[\text{app}(i,j)+\text{loc(i,j)}]; & \text{otherwise} \end{matrix} \right}.$$
#####
