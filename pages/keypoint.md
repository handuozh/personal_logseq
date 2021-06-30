---
title: keypoint
alias: feature
permalink: keypoint
public: true
---

- Survey
  id:: 5fffb4b2-afcd-448a-8399-c470f75286ac
	- 2019
		- 链接：https://www.zhihu.com/question/32066833/answer/511051905
		- 还是和之前一样先说{{alias:[[Keypoint Extraction]]Keypoint Detector}}。不出意外，现在几乎都是detector和descriptor一起去学了，这个任务进展非常大。个人觉得比较有意思的几个工作：首先是CVPR'19的[[D2-Net]](https://github.com/mihaidusmanu/d2-net)，这篇大意是说detector其实没必要特意去学，直接从descriptor里取depth-wise maximum和local spatial maximum就可以得到。如果沿着这个方向做，甚至detector都没必要特意设计loss去学（这个loss真的挺难设计的），确实是个很吸引人的性质。但现在d2-net的keypoint localization还是很不准，这对geometry computation影响还是很大，所以如果用来做sfm或者slam这种对geometry很敏感的任务估计还不太行。最近还有一篇[[R2D2]] (https://arxiv.org/abs/1906.06195) 会比d2-net复杂一些，不过看实验结果似乎localization error小了很多，如果以后开源可以多测下。不过D2-Net和R2D2都依赖dense gt correspondences, 比如D2-Net依赖MegaDepth，而R2D2是用EpicFlow自己插值出来的，获取成本和精度都是麻烦事...所以最近有一篇[[Unsuperpoint]]，完全[[Self-supervised]]方法只用homography transformation（这点其实和[[Superpoint]]很像，不过做成了self-improving，比[[Superpoint]]优雅）去做，看起来效果也相当不错，期待它开源了再仔细测一测了。
		- 然后回到[[Descriptor Extraction]]。这次CVPR关于descriptor有两篇oral工作，一篇是[SOSNet](https//github.com/yuruntian/SOSNet)，引入了second order的[[regularization]]，没有复杂的trick，感觉应该是简单有效的。另外是我们组的工作[ContextDesc](https://github.com/lzx551402/contextdesc)。做这篇的出发点很简单：local patch能提供的信息是有限的，而对于常见的repetitive pattern，很多时候这些patch就不是locally discriminative的，再好的descriptor可能也没法提供更有价值的信息。最直观的想法，当然是forward更多的visual information，比如增大receptive field，但有没有更高效的方法？如何去平衡local detail和global context两者的表示，还是有不少问题值得探索的。另一方面是说，spatial information有什么可以利用的？keypoint location又能提供什么线索？这篇工作的出发点就是去利用更多的contextual information，而不仅仅去依赖local visual information，去增强现有的descriptor，而非学一个新的descriptor。
		- 最后仍然说下matching。learning-based outlier rejection在这一年里也几篇跟进，包括CVPR'19的[NM-Net](https://arxiv.org/abs/1904.00320) (oral), 以及刚挂上arxiv的一篇kwang moo yi组的[续作](https://arxiv.org/abs/1907.02545)。我们组ICCV也做了一篇[OA-Net](https://arxiv.org/abs/1908.04964)。其实三篇工作的核心思想都有些共通，就是要做一些clustering，去capture更多的local motion信息而不仅仅是单纯global的信息。在CVPR19的两个workshop（[Local Features & Beyond](https://image-matching-workshop.github.io/leaderboard/)和[Long-term Visual Localizatio](https://www.visuallocalization.net/workshop/cvpr/2019/)n）里，我们把ContextDesc和OA-Net做了结合，也取得了相当不错的效果。
	- 2018
		- 首先是detection, 这一块的文章相对较少，因为通常你很难给出一个对keypoint的清晰定义，这样你就不太容易去用一些supervised的方法去进行训练。其中我觉得比较有意思的几篇 Quad-Net ([https://arxiv.org/abs/1611.07571], [[Superpoint]]([Self-Supervised Interest Point Detection and Description](https://arxiv.org/abs/1712.07629)), [[Lf-net]] ([Learning Local Features from Images](https://arxiv.org/abs/1805.09662))， 其中后两篇是end-to-end带着descriptor一起学的，未来应该也会有进一步的发展。对于这个任务，我觉得最大的问题是learning-based methods收益到底能有多大，因为传统的诸如DoG已经有比较完备的理论支撑(scale space theory)，而且适应于绝大部分场景。在这样一个非常基础的task上消耗如此多计算资源，到底收益如何？generalization能力怎样？我觉得是两个非常实际的问题。
		- 之后是description. 这一块文章就很多了，从improve invariance property的角度来说，deep feature确实更有可能超过诸如SIFT这种基于image gradient的策略。这几年我觉得比较有突破性的工作，一个是L2-Net ([yuruntian/L2-Net](https://link.zhihu.com/?target=https%3A//github.com/yuruntian/L2-Net))， 一个是HardNet ([DagnyT/hardnet](https://link.zhihu.com/?target=https%3A//github.com/DagnyT/hardnet))，同时CVPR'18也有好几篇关于这个的paper。另外也安利一下我们组ECCV'18的工作GeoDesc（[lzx551402/geodes](https://link.zhihu.com/?target=https%3A//github.com/lzx551402/geodesc)），俗话说百闻不如一见，我们直接提供了测试脚本可以对SIFT进行对比。GeoDesc也被集成到了我们自己用于生产的3D reconstruction pipeline （[Altizure The Portal for Realistic 3D Modeling](https://www.altizure.com/)），并做了大量的测试，所以我还是有些信心说这个工作是很实用的吧
		- 最后是matching。基于3d point cloud进行matching的文章近年很多，（也有我们的一篇ECCV'18的工作顺便安利下~[zlthinker/RMBP](https://github.com/zlthinker/RMBP)），但2d matching改进不是很大，还是传统的那一套：nearest neighbor matching, outlier rejection (e.g., ratio test), geometric verification (e.g., RANSAC)。但CVPR'18有篇很有意思的文章([[1711.05971] Learning to Find Good Correspondences](https://arxiv.org/abs/1711.05971)) 做outlier rejection这一步，虽然paper细节我还有些疑问，但总体来说是极少数从raw 2d point set上extract feature的工作，而且把geometric verification给encode进网络里也非常有意思
- [[D2-Net]]
- [[R2D2]]
- [[GCNv2]]
- [[ContextDesc]]
- [[ASLFeat]]
- [[Average-Precision (AP)]]
- [[Unsuperpoint]]
- [[KP2D]]
- [[LISRD]]
- [[DISK]]
-