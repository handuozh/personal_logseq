---
title: RANSAC
alias: Random Sample Consensus
---

- [[outlier rejection]], [[visual odometry]]
- 随机抽样一致
    - an iterative method to estimate parameters of a maths **model** from a set of observed data with outliers.
    - A basic assumption is that the data consists of "**inliers**",
        - data whose distribution can be explained by some set of model parameters, though may be subject to noise
        - outliers are data that do not fit the model.
- 通过iteratively反复选择数据中的一组随机 [[subset]] 达成目标
    - 被选取的子集假设为局内点
- ![image.png](/assets/pages_ransac_1611212053255_0.png)
    - distance threshold $t$ usually chosen empirically
    - [[multi-view geometry]]  page 118-120
- Application
    - matching of image features -> estimate [[Homography matrix]] $H$
-