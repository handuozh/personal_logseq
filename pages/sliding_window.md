---
title: sliding window
alias: active window
tags: [[Optimization]]
---
## In order to achieve constant-time operation in visual SLAM, we can dynamically define a sub-set $M_{\mathcal{A}}$ of all $M$ keyframes as the active window $\mathcal{A}$  over which to apply optimization

## Usually most recently captured frames
## [[keyframe]]s are fixed at the boundary of the active window
### all thos keyframes with co-visible points are included
## Works well in exploratory situations
### with large but few loops
### For a loop camera motion, the number of keyframes at the boundary is large w.r.t. the total number of keyframes within the [[active window]].
#### fixing them hampers convergence