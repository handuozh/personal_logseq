---
title: video instance segmentation tracking with VAE
tags: [[VAE]] [[segmentation]] tracking
public: true
---

- TODO  To read!
  todo:: 1608191817524
- Structure: 1 encoder, 3 parallel branches
  heading:: true
	- auxiliary branch
		- predict future frame
		- reconstruct loss
		- learn finer representations, meaningful semantic information encoded in latent space
	- proposal branch
		- object-level information for connecting objects over time
		- attentive cues to reduce false negatives in augment branch
	- augment branch
		- aggregates pixel-level features extracted from different layers in encoder
- [[https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_12_17_seg_track.png?Expires=4761774928&Signature=bWAasSKsLNWPwdakaI0qyeOScAQZ0RrL7iaixe744FvvyLI6uit11uyh5Lee9GYnj-fuRRTrTgZ4-g78VcfP-AQPt5HeAXR85c0y-SZdIx1tewePik6cwTuytZczGKhhNGj8x9GQCds~jg0vKfZR0YY57PSG-4gb8nz1lqrPfQp1ZGsfI9j2NLawAvuQe9WKpipDkg6V32o7~K5aWPLeO5tqCXaA6h~0r6SoU44BmziOxJU2WYjirtki~g~NcP53Vxu5F3wsy5BD~~nKRe5FPqP812zLPcDQwaf3iUmurV1XPwySBJENsvNstoMJJYwIF50C0DohBxI2vHelj85wrg__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA][2020_12_17_seg_track.png]]
- Unified [[VAE]]
  heading:: true
	- Video sequence $F$ with $T$ frames $F_t, t\in{\{1, \cdots, T\}}$ with $N$ instances belonging to label set $C$.
	- Input
	  heading:: true
		- Current observation $\xi_t=[F_t,F_{t-1}, \Lambda_{t-1}]$
	- Structure
	  heading:: true
		- Encoder $E_{\phi}$
			- maps the current observation $xi_t$ to:
				- a latent variable $\mathbf{z}$ and
				- a spatial prior $\mathbf{\phi}$
		- Auxiliary Decoder $D_{\theta}^{aux}$
		- Proposal Decoder $D_{\theta}^{pro}$
		- Augmentation Decoder $D_{\theta}^{aug}$
	- Output $\hat{\chi}_t=[\hat{F}_{t+1},\hat{\Gamma}_{t},\hat{\Lambda}_t]$
	  heading:: true
		- Predict future frame $\hat{F}_{t+1}$ in $D_{\theta}^{aux}$
		- Generate a set of detectioni box predictions $\hat{\Gamma}_{t}$ in $D_{\theta}^{pro}$
			- $\hat{\Gamma}_{t}=\{b_{i,t}\}^{n_b}_{i=1}$
		- Estimate a set of instance segmentation masks $\hat{\Lambda}_t$ in $D_{\theta}^{aug}$
			- $\hat{\Lambda}_t=\{m_{i,t}\}_{i=1}^{n_m}$
		- where $b_{i,t}$ and $m_{i,t}$ is detection box and segmentation mask for instance $i$ at frame $t$.
		- $n_b$ and $n_m$ are number of object instances at frame $t$ in $D_{\theta}^{pro}$ and $D_{\theta}^{aug}$
	- 1. Conditional Variational Bound
	  heading:: true
		- Add a conditional prior $\varphi$ extracted from $\xi$ to preserve spatial information
		- $D_{\theta}$ estimates the parameters of distribution $p_{\theta}(\chi_t|z,\varphi)$
			- We need to maximize the **log-likelihood** of observed data $\xi$
			- **marginalize out** the latent variables $z$ and $\varphi$.
		- Use approximate posterior $q_{\phi}(z|\xi,\varphi)$ to obtain the [[ELBO]] from [[Jensen's inequality]]
			- ^^(1)^^ $\log{p_{\theta}(\chi_t|\xi)}\geqslant \mathbb{E}_q \log{\frac{p_{\theta}(\chi_t|z,\phi)p_{\phi}(z|\varphi)}{q_{\phi}(z|\chi,\varphi)}}.$
			- The loss function follows (1) and has the form:
				- $\mathcal{L}(\chi_t,\theta, \phi)=-D_{KL}\left(q_{\phi}(z|\xi,\varphi)||p_{\theta}(z|\varphi)\right) + \mathbb{E}_{q_{\phi}(z|\xi,\varphi)}[\log{p_{\theta}(\chi_t|z,\varphi)}].$
					- $D_{KL}$ divergence, a **latent** loss as distance between distribution $q_{\phi}(z|\xi,\varphi)$ and prior distribution $z$.
						- As [[regularization]] term to avoid departs too much from prior.
					- log-likelihood part, a **decoding** loss, which measures how accurately the network constructs the semantic output $\chi_t$ by using the distribution $p_{\theta}(\chi_t|z,\varphi)$
						- distance between $\hat{\chi}_t$ and $\chi_t$.
	- 2. Variational Inference with Gaussian Process Latent Variables
	  heading:: true
		- Video data strong spatial correlation
		- So to relax the assumption that latent variables are [[iid]], propose new scheme
			-