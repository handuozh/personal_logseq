public:: true
citekey:: khoslaSupervisedContrastiveLearning2021
authors:: [[Prannay Khosla]], [[Piotr Teterwak]], [[Chen Wang]], [[Aaron Sarna]], [[Yonglong Tian]], [[Phillip Isola]], [[Aaron Maschinot]], [[Ce Liu]], [[Dilip Krishnan]]
tags:: #cross-entropy, [[contrastive learning]], #supervised, #zotero, #literature-notes, #reference
publication-date:: [[2021-03-10]]
type:: [[Article]]

- Supervised Contrastive Learning #toread
	- URL: [http://arxiv.org/abs/2004.11362](http://arxiv.org/abs/2004.11362)
	- PDF Attachments
		- [Khosla et al_2021_Supervised Contrastive Learning.pdf](zotero://open-pdf/library/items/R4RFG4Z5)
		- [Local library](zotero://select/items/1_E5Y5MSGS)
	- Abstract
		- Contrastive learning applied to [[Self-supervised]] representation learning has seen a resurgence in recent years, leading to state of the art performance in the [[Unsupervised-learning]] training of deep image models.
		- Modern **batch** contrastive approaches subsume or significantly outperform traditional contrastive losses such as triplet, max-margin and the N-pairs loss.
		- In this work, we extend the _self-supervised batch contrastive_ approach to the ^^fully-supervised setting^^, allowing us to effectively leverage label information.
			- Clusters of points belonging to the same class are pulled together in ^^embedding space^^, while simultaneously pushing apart clusters of samples from different classes.
			- We analyze two possible versions of the supervised contrastive (SupCon) loss, identifying the best-performing formulation of the loss.
		- Performance
			- On ResNet-200, we achieve top-1 accuracy of 81.4% on the ImageNet dataset, which is 0.8% above the best number reported for this architecture.
			- We show consistent **outperformance over cross-entropy** on other datasets and two ResNet variants. The loss shows benefits for robustness to natural corruptions and is more stable to hyperparameter settings such as optimizers and data augmentations.
			- Our loss function is simple to implement, and reference TensorFlow code is released at https://t.ly/supcon.
-