---
title: optical expansion
public: true
type: Article
publication date: 2020
publication press: [[CVPR]]
tags: [[Optical Flow]] 
---

- Upgrading optical flow to 3d scene flow through optical expansion
- Optical Expansion vs [[Optical Flow]]
    - ![image.png](/assets/pages_optical_expansion_1610542666164_0.png)
    - [[Optical expansion]]:: 对于一个区域,在前后两帧之间,面积发生变化的比例
        - 例如c图中，中间的大蓝色矩形的面积，与左上角小蓝色矩形的面积的比值，即为这个矩形的optical expansion
        - 一个物体的optical expansion大于1，说明在3D空间中，物体在向相机靠近；如果小于1，则其在远离相机.
- Optical Expansion v.s. Motion in Depth
    - 如下场景
        - ![image.png](/assets/pages_optical_expansion_1610601931342_0.png)
        - 倒数的关系
- Normalized Scene Flow
  heading:: true
    - motion-in-depth $\tau$ 结合intrinsics $K$ 计算normalized 3D scene flow vector
    - ((5ffeb0b3-fc27-46ef-8016-74cebe53cc76))
    - 3D scene flow $\mathbf{t}$:
        - Relative to camera
        - factor out camera motion
        -
          $$ \begin{aligned} \mathbf{t} &= \mathbf{P}^{\prime} - \mathbf{P} \\ &= \mathbf{K}^{-1}(Z^{\prime}\tilde{\mathbf{p}}^{\prime}-Z\tilde{\mathbf{p}})  \\ &= Z\mathbf{K}^{-1}[\tau(\tilde{\mathbf{u}}+\tilde{\mathbf{p}})-\tilde{\mathbf{p}}] \;\;\; \rm{where} \; \; \tilde{\mathbf{u}}=\tilde{\mathbf{p}}^{\prime} - \tilde{\mathbf{p}} \\ 
          &= Z\mathbf{K}^{-1}[(\tau-1)\tilde{\mathbf{p}}+\tau \tilde{\mathbf{u}}] \\ &=Z\hat{\mathbf{t}}  \; \; \;\;\; \rm{where} \; \; \; \hat{\mathbf{t}}=\mathbf{K}^{-1}[(\tau-1)\tilde{\mathbf{p}}+\tau \tilde{\mathbf{u}}] \end{aligned}$$
        - denote $\hat{\mathbf{t}}$ as [[normalized scene flow]]
            - ((5fffe3b2-f45c-48f4-8e43-48db5120bbbe))
- Learning normalized scene flow
  heading:: true
    - 两种办法 [[supervised]] fashion 和 [[Self-supervised]]  learning
    - End-to-end network
        - estimating [[normalized scene flow]]总共分三步
            - (1) [[Optical Flow]] estimation -> $(u, v)$
            - (2) [[optical expansion]] estimation -> scale $s$
            - (3) [[motion-in-depth]] estimation $\tau$
                - optical expansion is refined to produce correct outputs for a full perspective camera model
        - [[Local Affine Layer]]
          heading:: true
            - **目的**是提取**dense** optical expansion
            - 对于2D图片中的任何一个像素，以其为中心，抠出来一个$3\times 3$的patch
                - 假设这个patch是一个刚体（rigid body）的一部分，并且平行于像平面
                - 假设在前后两帧图像中，这个patch是平行于像平面运动的
            - (1) Fit local affine motion models  _(dense layer)_
                - Given a dense optical flow field $\mathbf{u}$ over a reference frame and a target frame
                - fit a local [[affine transform]] $\mathbf{A}\in{\mathbb{R}^{2\times 2}}$ for each pixel $\mathbf{x}_c=(x_c,y_c)$ over its $3\times 3$ neighborhood $\mathcal{N}(\mathbf{x}_c)$ in the reference image
                    - by solving by [[least squares]]
                        -
                          $$(\mathbf{x}^{\prime}-\mathbf{x}^{\prime}_c)=\mathbf{A}(\mathbf{x}-\mathbf{x}_c), \; \mathbf{x}\in{\mathcal{N}(\mathbf{x}_c)}$$
                        - 这里 $\mathbf{x}^{\prime}=\mathbf{x}+\mathbf{u(x)}$ 是$\mathbf{x}$匹配点
            - (2) Extract expansion _(pixel-wise layer)_
                - compute optical expansion of a pixel as the ^^ratio^^ of the **areas** between the deformed vs original $3\times 3$ grid
                    - 这个仿射矩阵的[[rank]]就是patch的optical expansion
                    -
                      $$s=\sqrt{|\rm{det}(\mathbf{A})|}$$
            - (3) Compute fitting errors _(differential layer)_
                - compute **residual** $L_2$ error of the [[least squares]] fit
                - pass it as an additional channel to the optical refinement network
        - Learn expansion (**supervised**)
            - challenge: ground-truth
                - Common的方法: search over a [[multi-scale image pyramid]]是不可能的(因为结果sparse and inaccurate)
            - For each pixel with optical flow ground-truth, we fit an [[affine transform]] over $7\times 7$ neighborhood
                - extract the scale component, similar to [[local affine layer]]
                - 抛弃高error的pixels
        - Learn expansion ( [[Self-supervised]])
        - Learn [[motion-in-depth]]
    - 过滤条件
        - $s>0.5$
        - $s < 2$
        - $error < 0.1$
    -