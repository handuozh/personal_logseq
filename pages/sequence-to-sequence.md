---
title: sequence-to-sequence
alias: Seq2Seq
related: [[attention]] 
area: [[NLP]] 
---

- Input and output are both vector sequences
- [[Encoder-decoder]] strucuture
    - [seq2seq model](https://i.imgur.com/0v6b9d8.png)
    - 缺点: final state cannot remember a **long** sequence
        - the decoder looks at only ^^its current state^^
          id:: 602fb310-8bf7-4748-8f48-d08f07aacc17
    - $O(m+t)$ time complexity
    - [[BLEU]] score是评价机器翻译好坏的标准
    - So need [[attention]]
        - [[seq2seq]] model does not forget source input
        - [[decoder]] knows where to _focus_
- Commonly used in machine translation, image caption and [[NLP]]
    - [[RNN]]
    -