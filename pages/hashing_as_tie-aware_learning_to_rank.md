---
title: Hashing As Tie-aware Learning To Rank
public: true
---

- Meta Data
    - #title Hashing as Tie-Aware Learning to Rank, 2018
    - #topic #nearest-neighbour #Ranking
- Abstract
    - 主要解决的是在信息检索或是[[hashing]]的过程中遇到的相同海明距离相同排名的问题，检索的相同排名的其实对结果有很大影响，很多人都没有意识到这一点，采用一些随机的或是无法公式化的方法来打破相同排名
    - ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FSLAM%2FIUoqVFoNB3.png?alt=media&token=e4a09b16-a827-4781-8220-e3129b6781cc)
    - ties:: items that share the same distance
        - 相同的tie可能有相当不同的evaluation metric，比如[[AP]]
    - tie-aware ranking metrics implicitly average over all permutations in closed form.
    - 文章介绍AP([[Average-Precision (AP)]]) 和[[MAP]]: https://makarandtapaswi.wordpress.com/2012/07/02/intuition-behind-average-precision-and-map/
    - 对于[[Pointwise]]，[[Pairwise]]，[[Listwise]] ，在论文的intrduction部分我第一次看到了Listwise这个词，之后查阅资料发现相当于信息检索的样本不同，[[Pointwise]]是完全从单文档的角度考虑，没有考虑多个文档间的顺序，[[Pairwise]]是将一个文档对作为训练样本，任何两个不同label的文档，都可以得到一个训练实例（di,dj），如果di>dj则赋值+1，反之-1，于是我们就得到了二元分类器训练所需的训练样本了，但这个只考虑了两两之间样本的相对顺序，没有整体性的考率
        - [[Listwise]]是将每个查询所有的搜索结果看成一个训练样本经行训练，[[Listwise]]根据训练样例训练得到最优评分函数F，对应新的查询，评分F对每个文档打分，然后根据得分由高到低排序，即为最终的排序结果
          id:: 5fbf5c9c-c6b0-4c2f-ad73-31f65deb8b64
    - Architecture
        - ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FSLAM%2F4WkYUA-eQ1.png?alt=media&token=95ee5bdd-6091-418b-82e6-c5033cb870af)