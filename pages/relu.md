---
title: ReLU
---

## A recent invention which stands for **Rectified Linear Units**. The formula is deceptively simple: $$\max(0,z)$$. Despite its name and appearance, it’s not linear and provides the same benefits as [[Sigmoid]] but with better performance.
## ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FSLAM%2FIjmPuHK1KZ.png?alt=media&token=b7191d02-c6c5-4e99-9817-08f2ef256b46)
## Pros
    - It avoids and rectifies vanishing gradient problem.
    - **ReLU** is less computationally expensive than [[tanh]] and [[Sigmoid]] because it involves simpler mathematical operations.
## Cons
### One of its limitations is that it should only be used within ^^Hidden layers^^ of a Neural Network Model.
### Some gradients can be fragile during training and can die. It can cause a weight update which will make it never activate on any data point again. Simply saying that ReLu could result in ^^Dead Neurons^^.
### In other words, For activations in the region (x<0) of ReLu, the gradient will be 0 because of which the weights will not get adjusted during descent. That means those neurons which go into that state will stop responding to variations in error/ input ( simply because the gradient is 0, nothing changes ). This is called **dying ReLu problem**.
## The range of ReLu is [0, inf). This means it can blow up the activation.
