---
title: Attention is all you need
type: Article
citekey: vaswaniAttentionAllYou
authors: [[Ashish Vaswani]], [[Noam Shazeer]], [[Niki Parmar]], [[Jakob Uszkoreit]], [[Llion Jones]], [[Aidan N Gomez]], [[Łukasz Kaiser]], [[Illia Polosukhin]]
tags: [[transformer]], [[attention]], #reference, [[literature-notes]]
---

- Attention is All you Need #readdone
        - Topics: [[robotic centric mapping]]
        -
        - #zotero #literature-notes #reference
        - PDF Attachments
    - [Vaswani et al_Attention is All you Need.pdf](zotero://open-pdf/library/items/LV4AMHPH)
        - [[abstract]]:
          The dominant sequence transduction models are based on complex recurrent or convolutional neural networks that include an encoder and a decoder. The best performing models also connect the encoder and decoder through an attention mechanism. We propose a new simple network architecture, the Transformer, based solely on attention mechanisms, dispensing with recurrence and convolutions entirely. Experiments on two machine translation tasks show these models to be superior in quality while being more parallelizable and requiring signiﬁcantly less time to train. Our model achieves 28.4 BLEU on the WMT 2014 Englishto-German translation task, improving over the existing best results, including ensembles, by over 2 BLEU. On the WMT 2014 English-to-French translation task, our model establishes a new single-model state-of-the-art BLEU score of 41.0 after training for 3.5 days on eight GPUs, a small fraction of the training costs of the best models from the literature.
        - zotero items: [Local library](zotero://select/items/1_XGLQP6PG)
- 最大的贡献是抛弃了传统的[[CNN]]和[[RNN]],将 [[attention]] 机制发挥到底,使整个网络结构完全是attention机制组成
    - 解决[[RNN]], [[LSTM]], [[GRU]]无法并行训练的问题
        - 只能从左到右依次计算
        - 长依赖信息丢失的问题,顺序计算过程中信息还是会丢失
-