---
title: feed forward network
alias: FFN,前馈神经网络
---
## Used in [[transformer]]
## code
###
```python
class PositionwiseFeedForward(nn.Module):
    ''' A two-feed-forward-layer module '''

    def __init__(self, d_in, d_hid, dropout=0.1):
        super().__init__()
        # 两个fc层，对最后的512维度进行变换
        self.w_1 = nn.Linear(d_in, d_hid) # position-wise
        self.w_2 = nn.Linear(d_hid, d_in) # position-wise
        self.layer_norm = nn.LayerNorm(d_in, eps=1e-6)
        self.dropout = nn.Dropout(dropout)

    def forward(self, x):
        residual = x

        x = self.w_2(F.relu(self.w_1(x)))
        x = self.dropout(x)
        x += residual

        x = self.layer_norm(x)

        return x
```
###
## [[DETR]]
### 3-layer perceptron with [[ReLU]] activation and a **hidden dimension** $d$, and a linear projection layer
### Predict the normalized center coordinate, height, width of box
#### linear projection layer predicts the class label using [[softmax]] function
