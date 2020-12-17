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
### Video sequence $F$ with $T$ frames $F_t, t\in{\{1, \cdots, T\}}$ with $N$ instances belonging to label set $C$.
### Structure
#### Encoder $E_{\phi}$
#### Auxiliary Decoder $D_{\theta}^{aux}$
#### Proposal Decoder $D_{\theta}^{pro}$
#### Augmentation Decoder $D_{\theta}^{aug}$
