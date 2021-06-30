---
title: pytorch
---

- Dataloader
    - DatasetFolder
        - 属于[[torchvision]]
        - 例子
            -
              ```python
              from torchvision.datasets import DatasetFolder
              from pathlib import Path
              # I have text files in this folder
               ds = DatasetFolder("~/data/dataset01/my_text_dataset", 
                loader=lambda path: Path(path).read_text(),
                extensions=(".txt",), #only load .txt files
                transform=lambda text: text[:100], # only take first 100 characters
              )
              
              # Everything you need is already there
              len(ds), ds.classes, ds.class_to_idx
              (20, ['novels', 'thrillers'], {'novels': 0, 'thrillers': 1})
              ```
            - 如果你在处理图像，还有一个`torchvision.datasets.ImageFolder``类，它基于[[DatasetLoader]]，它被预先配置为加载图像
            -