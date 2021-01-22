---
title: Coppersmith-Winograd
---

## 步骤

### 1. 首先把输出通道按照6x6的大小做补齐，使得整体的大小为6x6的整数倍，填充后的输出再加一层padding
### 2. 处理底层feature map层
#### 把底层复制成扩展后的输出层+边的大小,空出来的地方填充V值
####
```python
copy_make_border(bottom_blob, bottom_blob_bordered, 0, h - bottom_blob.h, 0, w - bottom_blob.w, 0, 0.f);
```
###
