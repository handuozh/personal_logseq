- #related
  - [[Mask R-CNN]]
- 以便使生成的候选框region proposal映射产生固定大小的feature map
  - 同时解决ROI pooling中量化数据损失的问题
- ![image.png](assets/image_1621304600370_0.png)
  -
    1. Conv layers VGG16, feat stride=32
    - input $800\times 800$
    - output $25\times 25$
  -
    2. Assume region proposal $665\times 665$
    - $665/32=20.78$ 不是整数
    - 不像 [[ROI pooling]] 一样取整，而是保留float
  -
    3. Assume pooled width=7, pooled height=7,即pooling后固定为$7\times 7$大小的特征图
    - 将feature map上映射的$20.78\times 20.78$的region proposal划分为49个小区域
    - 每个区域大小为$20.78/7=2.97$,即$2.97\times 2.97$
  -
    4. Assume 采样点数为4,即表示，对于每个$2.97\times 2.97$的小区域，平分4份，每份取其**中心点**位置
    - 中心点位置的像素，采用 [[bilinear interpolation]] 进行计算，得到四个点的像素值
    - ![2021-05-18_10-45.png](assets/2021-05-18_10-45_1621305916703_0.png)
    -