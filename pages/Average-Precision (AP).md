---
title: Average-precision (AP)
public: true
tags: #Keypoint, #[[Descriptor-Matching]],  #Ranking, #metric-learning, #[[nearest-neighbour]]
---
### A standard ranking metric in paper _Local descriptors optimized for average precision. In CVPR, 2018. The paper focuses on [Descriptor Matching](Descriptor-Matching.md) stage.
## metric-learning
### ((5fbf5a26-4912-470d-b02b-594f29bcf4f7))
## Formulate descriptor extraction as [[nearest-neighbour]] retrieval performance metric: Average Precision.  

## Estimating fudamental matrix from matching images
:PROPERTIES:
:heading: true
:END:
### 1) [[Keypoint Extraction]] detect and extract $$ M $$ local features from each image.
### 2) [[Descriptor Extraction]]
### 3) [[Descriptor-Matching]]::
#### Compute pairwise distance matrix with $$ M^2 $$ entries.
:PROPERTIES:
:id: 5fbf59b6-8148-4ff9-8d56-d9916c2c9189
:END:
#### For each feature in $$ I_1 $$ looks for nearest neighbor in $$ I_2 $$ and ^^vice versa^^.

#### Feature pairs that are mutual nearest neighbors are candidate matches.

### 4) Robust Estimation
## [[https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_11_26_ap.png?Expires=4759976180&Signature=Vlk86f11suQTus8Hzbe1r84KbQtm4oegdQTL1mvcaB30ZxRLetg~W9PikLxP34LIYJfUdDaSWzOpaaRxrXptr5anqjOuMfQI~kciXVri39~K7Qc3SWn79uNQa7fGeK93q1gfVkoH3Cmh5j1Tt8xkae1bmDOY~3PwFzXczySOy6f4ies9xcM8wLykZpbSWLw0h840wEG6H9utNIGLD0ilBvbmw1A5qWs-lnSEf0OdCo9l63e6bo9p4ZLxeeQO08~kboiPzgrt~pW67mmSgTqPbo2KRlrwhLlh7xC3Zib9KJJWx1OXJR38MOD8gsmq07GgRAhV90OJ-xLN8cyRMxCJjg__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA][2020_11_26_ap.png]]
## In this paper the ^^contribution^^ is to assess [[nearest-neighbour]] matching performance by adopting Average Precision ([[AP]]) as the evaluation metric, based on:
### Binary relevance assumption
#### retrievals are either "relevant" or "irrelevant" to the query.
### So given a reference feature, features in a target image are either **true match** or **false match**.

## 1. **Optimizing Average Precision**
:PROPERTIES:
:heading: true
:END:
### 1.1 Math notation
#### Let $$ \mathcal{X} $$ be the space of image patches and $$ S \sub \mathcal{X} $$ be a database.

#### For a query patch $$ q\in \mathcal{X} $$, let $$ S_q^+ $$ be the set of matching patches in $$ S $$, $$ S_q^- $$ the set of non-matching patches.

#### Given a distance metric $$ D $$, let $$ (x_1,x_2,\cdots,x_n) $$ be ranking of items in $$ S_q^+ \cup S_q^- $$ sorted by increasing distance to $$ q $$.

#### TODO Quantization of sampled scores via [[1d Convoluation]] to increase channel number (1 to 40). 

#### Given ranking, AP is the average of precision values $$ P(K) $$ evaluated at different positions:
##### (1)     $$ P(K)=\frac{1}{K}\sum\limits_{i=1}^{K}\mathbf{1} \left[  x_i\in S_q^+  \right] $$ ,

#####
```python
# number of samples  N x Q = c
nbs = q.sum(dim=-1)
 # nb of correct samples = c+ N x Q
rec = (q * label.view(N, 1, M).float()).sum(dim=-1)
 # precision
prec = rec.cumsum(dim=-1) / (1e-16 + nbs.cumsum(dim=-1))
```
##### (2)      $$ AP=\frac{1}{|S_q^+|}\sum\limits_{K=1}^n \mathbf{1} \left[ x_K \in S_q^+   \right] P(K) $$ 
where $$ \mathbf{1}\left[ \cdot \right] $$ is the binary indicator.

#####
```python
  # norm in [0,1]
rec /= rec.sum(dim=-1).unsqueeze(1)
  # per-image AP
ap = (prec * rec).sum(dim=-1)
```
#### [[AP]] achieves its optimal value ^^if and only if^^ every patch from $$ S_q^+ $$ is ranked above all patches from $$ S_q^- $$.
#### AP optimization is a [[metric-learning]] problem with ^^goal^^ to learn a distance metric $D$ that give optimal [[AP]] when used fro retrieval.
:PROPERTIES:
:id: 5fbf59b6-d9a1-4ecf-bd22-a9f151e36be6
:END:
#### ^^Challenge^^ is that: sorting operation is [[Non-differentiable]], and continues changes in the input distances -> discontinuous "jumps" in AP value.
#### We thus need [[Smoothing]], based on [[Hashing As Tie-aware Learning To Rank]] paper, we employ a differentiable approximation to [[Histogram Binning]] to optimize ranking-based objectives with gradient descent.

#### TODO Read the two papers mentioned above.

#### The optimization uses quantization based approximation.

####
```python
quantizer = q = nn.Conv1d(1, 2 * nq, kernel_size=1, bias=True)
  # quantize all predictions
q = self.quantizer(x.unsqueeze(1))
  # N x Q x M
q = torch.min(q[:, :self.nq], q[:, self.nq:]).clamp(min=0)
 ```
### 1.2 [[Binary Descriptors]]
#### compact storage and fast matching (in applications with speed or storage restrictions) #definition

#### In this paper they use gradient-based relaxation method to learn ^^fixed-length^^ [[Hash Codes]].

#### network $$ F $$ models mapping from patches to [[Low-dimensional]] [[Hamming Space]]: $$ F: \mathcal{X} \rightarrow {-1,1}^b $$. 

#### For the [[Hamming Distance]] $$ D $$, which takes integer values in $$ {0,1,\cdots,b} $$, [[AP]] can be computed in Closed Form using entries of a histogram $$ \mathbf{h}^+=(h_0^+,\cdots,h_b^+) $$,  where $$ h_k^+=\sum_{x\in{S_q^+}} \mathbf{1}\left[D(q,x)=k\right] $$. 

#### Approximation of [[Histogram Binning]]:

#### [[https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_11_26_ap1.png?Expires=4759976336&Signature=oEvDuE11U0Xgq-AilHDKBeMp0MaX9ega9xE1l7qnyv7Q7SRX1rIw15N1eRr~a7gcHnWZiAsmD7nAoj8uzXFWIzy-JpgPgsZBXXbu~ww2b~2WR3QwuoCgJ1gWBCtrElNCE4QA9TRcwE-WCIFClDutpAzJgdvBuQU0i4gXBJU01IgEgAz2l4KPpbobnF52M5sXnAFrIkCnFlTLgY8h6Yw1QVn~eamslfCYywf~R0o8Az-V7CRd6oOj8PuYHJxfZR20~phs1A6DG3tpaWw7T5NlwwoMaHu0vOIZUuR0-cYAxPr8kSnvc5k4YIsMA-9fSznVhV7bbt~frJQbdublA2i~LQ__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA][2020_11_26_ap1.png]]
### 1.3   [Real-Valued Descriptors](Real-Valued-Descriptors.md)

### Preferred in high-precision scenarios.

### Descriptors are modeled as a vector of real-valued network activations

### $$ L_2 $$ normalization applied to get Euclidean distance $$ D(x, x^{\prime})=\sqrt{2-2F(x)^{\top}F(x^{\prime})} $$ with $$ ||F(x)||=1 $$  

### Python Code:
####
```python
class APLoss (nn.Module):
  def __init__(self, nq=25, min=0, max=1, euc=False):
    nn.Module.__init__(self)
    assert isinstance(nq, int) and 2 <= nq <= 100
    self.nq = nq
    self.min = min
    self.max = max
    self.euc = euc
    gap = max - min
    assert gap > 0
    #init quantizer = non-learnable (fixed) convolution
    self.quantizer = q = nn.Conv1d(1, 2*nq, kernel_size=1, bias=True)
    a = (nq-1) / gap
    #1st half = lines passing to (min+x,1) and (min+x+1/a,0) with x = {nq-1..0}*gap/(nq-1)
    q.weight.data[:nq] = -a
    q.bias.data[:nq] = torch.from_numpy(a*min + np.arange(nq, 0, -1)) # b = 1 + a*(min+x)
    #2nd half = lines passing to (min+x,1) and (min+x-1/a,0) with x = {nq-1..0}*gap/(nq-1)
    q.weight.data[nq:] = a
    q.bias.data[nq:] = torch.from_numpy(np.arange(2-nq, 2, 1) - a*min) # b = 1 - a*(min+x)
    #first and last one are special: just horizontal straight line
    q.weight.data[0] = q.weight.data[-1] = 0
    q.bias.data[0] = q.bias.data[-1] = 1
  def compute_AP(self, x, label):
    N, M = x.shape
    if self.euc:  # euclidean distance in same range than similarities
    x = 1 - torch.sqrt(2.001 - 2*x)
    quantize all predictions
    q = self.quantizer(x.unsqueeze(1))
    q = torch.min(q[:,:self.nq], q[:,self.nq:]).clamp(min=0) # N x Q x M
    nbs = q.sum(dim=-1) # number of samples  N x Q = c
    rec = (q * label.view(N,1,M).float()).sum(dim=-1) # nb of correct samples = c+ N x Q
    prec = rec.cumsum(dim=-1) / (1e-16 + nbs.cumsum(dim=-1)) # precision
    rec /= rec.sum(dim=-1).unsqueeze(1) # norm in [0,1]
    ap = (prec * rec).sum(dim=-1) # per-image AP
    return ap
  def forward(self, x, label):
    assert x.shape == label.shape # N x M
    return self.compute_AP(x, label)
```
## **2. Comparison with Other Ranking Approaches**
### Some recent methods learn feature descriptors by optimizing losses defined on [[Triplets]] in the form of $(a,p^+,p^-)$ where $a$ is an anchor patch.
:PROPERTIES:
:id: 5fbf59b6-f639-4d14-8bf2-780ef2f3b88f
:END:
### Triplet loss:: Long history in [[metric-learning]]
#### Long history in [[metric-learning]], better suited for ranking tasks than pair-based losses used in [[Siamese Network]].

#### [[Triplets]] define local [[Pairwise]] ranking losses.
#### ^^Cons:^^ Despite simplicity, they are challenging to optimize. For$$ N $$ training samples, the set of [[Triplets]] is of size $$ O(N^3) $$. But most of them get classified well early during learning.

#### To maintain stable progress, carefully tuned heuristics such as [[Hard Negative Mining]], [[Anchor Swap]], [[Distance-weighted Sampling]] are crucial.  

### **Listwise ranking**:: defined on a ranked list.
#### {{{embed ((5fbf5c9c-c6b0-4c2f-ad73-31f65deb8b64)) }}}
### [[https://cdn.logseq.com/%2F0602f0ea-7667-4dfc-a07c-0cc047d72aaa2020_11_26_listwise.png?Expires=4759976476&Signature=MXEq4HWkrEbHWdFjNOpxNinuLyGBhXs28LvxKUEdYWhw4Jln~jikH4cPxMaE2jzn8ojfAijHsPQPEpQSTe-nFAwhqBObJQ~q~RlHRb3QVetkz7GOnTB92bTGcm-EDR24s5zvkogTtldvhkkVcFjLerryyoOhCOjHlL0spqEEMQbgsD-6P3TU8cPA8O637e6d8QK5-iPS7XCAm1WLkqmXr7X6Q3rbW53nFeJpH4bvJXlC9sPKRGV~u4~t05T6v9NkuCvh9GIHtTT96ZYhE3Eav4LVkp5o2unxuqtq-1FDYrPDMDcWiMkHavM21bBVsSqa2Q7CtuW6Y8T50tgTL-MG0g__&Key-Pair-Id=APKAJE5CCD6X7MP6PTEA][2020_11_26_listwise.png]]
### The listwise optimization also implicitly encodes [[Hard Negative Mining]]: it requires matching patches to be ranked above all non-matching patches, which automatically enforces correct classification of the hardest triplet in the batch without explicitly finding it.

## 3. Geometric Noise Handling

## 4. Label Mining for Image Matching