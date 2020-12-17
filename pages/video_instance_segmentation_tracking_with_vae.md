---
title: video instance segmentation tracking with VAE
---

## #topic  [[tracking]], [[segmentation]], [[VAE]]
## Structure: 1 encoder, 3 parallel branches
:PROPERTIES:
:heading: true
:END:
### auxiliary branch
#### predict future frame
#### reconstruct loss
#### learn finer representations, meaningful semantic information encoded in latent space
### proposal branch
#### object-level information for connecting objects over time
#### attentive cues to reduce false negatives in augment branch
### augment branch
#### aggregates pixel-level features extracted from different layers in encoder
## [[https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_12_17_seg_track.png?Expires=4761774928&Signature=bWAasSKsLNWPwdakaI0qyeOScAQZ0RrL7iaixe744FvvyLI6uit11uyh5Lee9GYnj-fuRRTrTgZ4-g78VcfP-AQPt5HeAXR85c0y-SZdIx1tewePik6cwTuytZczGKhhNGj8x9GQCds~jg0vKfZR0YY57PSG-4gb8nz1lqrPfQp1ZGsfI9j2NLawAvuQe9WKpipDkg6V32o7~K5aWPLeO5tqCXaA6h~0r6SoU44BmziOxJU2WYjirtki~g~NcP53Vxu5F3wsy5BD~~nKRe5FPqP812zLPcDQwaf3iUmurV1XPwySBJENsvNstoMJJYwIF50C0DohBxI2vHelj85wrg__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA][2020_12_17_seg_track.png]]
## Unified [[VAE]]
:PROPERTIES:
:heading: true
:END:
### Video sequence $F$ with $T$ frames $F_t, t\in{\{1, \cdots, T\}}$ with $N$ instances belonging to label set $C$.
### Input
:PROPERTIES:
:heading: true
:END:
#### Current observation $\xi_t=[F_t,F_{t-1}, \Lambda_{t-1}]$
### Structure
:PROPERTIES:
:heading: true
:END:
#### Encoder $E_{\phi}$
##### maps the current observation $xi_t$ to:
###### a latent variable $\mathbf{z}$ and
###### a spatial prior $\mathbf{\phi}$
#### Auxiliary Decoder $D_{\theta}^{aux}$
#### Proposal Decoder $D_{\theta}^{pro}$
#### Augmentation Decoder $D_{\theta}^{aug}$
### Output $\hat{\chi}_t=[\hat{F}_{t+1},\hat{\Gamma}_{t},\hat{\Lambda}_t]$
:PROPERTIES:
:heading: true
:END:
#### Predict future frame $\hat{F}_{t+1}$ in $D_{\theta}^{aux}$
#### Generate a set of detectioni box predictions $\hat{\Gamma}_{t}$ in $D_{\theta}^{pro}$
##### $\hat{\Gamma}_{t}=\{b_{i,t}\}^{n_b}_{i=1}$
#### Estimate a set of instance segmentation masks $\hat{\Lambda}_t$ in $D_{\theta}^{aug}$
##### $\hat{\Lambda}_t=\{m_{i,t}\}_{i=1}^{n_m}$
#### where $b_{i,t}$ and $m_{i,t}$ is detection box and segmentation mask for instance $i$ at frame $t$.
#### $n_b$ and $n_m$ are number of object instances at frame $t$ in $D_{\theta}^{pro}$ and $D_{\theta}^{aug}$
### Conditional Variational Bound
####
