---
title: positional encoding
alias: 位置编码
related: [[transformer]], [[BERT]]
---

- 传统的[[RNN]]中由于是以序列的模式逐个处理输入,所以顺序信息被天然地保存
- 对于 [[self-attention]] 乃至[[transformer]]需要额外的处理,把表示位置信息的编码添加到输入中
    - 表示两个单词$i$和$j$之间的距离
    - 这样词向量会加入单词的位置信息
- source: https://www.zhihu.com/question/347678607
- 理想状态下,编码方式需要满足
    - 对于每个位置的词语，它都能提供一个独一无二的编码
    - 词语之间的间隔对于不同长度的句子来说，含义应该是一致的
    - 能够随意延伸到任意长度的句子
- 有两种编码方式
    - 1. 网络自动学习
    - 2. 自定义规则
        - 比如sin-cos规则 比如
            - ((60482770-6d30-4ae8-8320-0ab267f9dde8))
            - positional encoding的表达式为有界的周期函数
                - $PE_{(pos, 2i)}=\sin (pos/10000^{2i/d_{model}})$
                - $PE_{(pos, 2i+1)}=\cos(pos/10000^{2i/d_{model}})$
                    - pos即$0$ to $N$，表示^^token^^在sequence中的位置
                    - $i$ 表示了Positional encoding的维度，$2i$的取值范围是$[0,\cdots, d)$
                    - $d_{model}$是输入单词的嵌入向量维度 $d$, feature map的第一维，比如256
        - 下一个位置的编码向量可以由前面的编码向量线性表示