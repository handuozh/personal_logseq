---
title: convolution
alias: 卷积
---

-
- Convolutional layer
  id:: 600a76a1-eb1d-4943-8788-d06d6549feee
    - cross-correlates of the input of a 3D Matrix, shaped like [height, width, channels] with [[convolution kernel]]s [k, k, channels] of weights (randomly initialized)
    - to produce an output
    - ![image.png](/assets/pages_convolution_1611282885532_0.png){:height 309, :width 602}
    - [[Padding]] 填充
    - [[Stride]] 步长
    -
- 1. 普通的Convolution
    - 1.1 No padding, no strides
        - ![no_padding_strides.gif](/assets/pages_convolution_1611297903090_0.gif){:height 177, :width 158}
    - 1.2 Arbitrary padding, no strides
        - ![arbitrary_padding_no_strides.gif](/assets/pages_convolution_1611303453251_0.gif){:height 200, :width 183}
    - 1.3 Half padding, no strides
        - ![same_padding_no_strides.gif](/assets/pages_convolution_1611303597725_0.gif){:height 215, :width 182}
    - 1.4 Full padding, no strides
        - ![full_padding_no_strides.gif](/assets/pages_convolution_1611303709888_0.gif){:height 222, :width 185}
    - 1.5 No padding, strides 2
        - ![no_padding_strides.gif](/assets/pages_convolution_1611303798668_0.gif){:height 175, :width 148}
    - 1.6 Padding, strides 2
        - ![padding_strides.gif](/assets/pages_convolution_1611303836489_0.gif){:height 179, :width 175}
    - 1.7 Padding, Strides
        - ![padding_strides_odd.gif](/assets/pages_convolution_1611303863357_0.gif){:height 167, :width 176}
    -
- 2. [[dilated conv]]
    - ![dilation.gif](/assets/pages_dilated_conv_1611304238899_0.gif){:height 197, :width 205}
    -