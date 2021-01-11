---
title: sequence-to-sequence
alias: Seq2Seq
---

## Input and output are both vector sequences
## [[Encoder-decoder]] strucuture
### [seq2seq model](https://i.imgur.com/0v6b9d8.png)
### 缺点: final state cannot remember a **long** sequence
### So need [[attention]]
#### [[seq2seq]] model does not forget source input
#### [[decoder]] knows where to focus
#### But downside is much more computation
#### Simple RNN + attention
##### **weight** $\alpha_i=align(\mathbf{h}_i, \mathbf{s}_0)$
##### **Context vector**: $\mathbf{c}_0=\alpha_1 \mathbf{h}_1 + \cdots + \alpha_m \mathbf{h}_m$
###### 1. linear maps
####### $\mathbf{k}_i=\mathbf{W}_K \cdot \mathbf{h}_i$ for $i=1$ to $m$
####### $\mathbf{q}_0=\mathbf{W}_{Q} \cdot \mathbf{s}_0$
###### 2. Inner Product
####### $\tilde{\alpha}=\mathbf{k}_i^{\top}\mathbf{q}_0$ for $i=1$ to $m$
###### 3. Normalization
#######
$$[\alpha_1, \cdots, \alpha_m]=\rm{Softmax}\left([\tilde{\alpha}_1, \cdots, \tilde{\alpha}_m]\right)$$
##### [attention+rnn](https://i.imgur.com/irWmoeR.png)
## Commonly used in machine translation, image caption and [[NLP]]
### [[RNN]]
###
