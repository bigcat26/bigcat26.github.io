---
title: Training YOLO
date: 2022-02-21 21:00:00
top_img: /img/tensorboard.jpg
cover: /img/tensorboard.jpg
tags: 
  - pytorch
  - dl
  - yolo
catagories:
  - 技术
  - 深度学习
---

# Learning Deep Learning

趁2022春节假期，再深入了解一下DL。这次目标是Train个YOLOv3。

最开始从Yolo v1开始看，感觉没有压力。然后直接跳到Yolo v5，觉得多了很多新概念一时之间没法理解，什么spp，anchor box的，只能回过头去看v3，就是yolo作者自己的最后一版。

Roadmap如下

1. 看懂Yolo网络结构
2. 代码复现
3. 用公开数据集train一个model
4. 成功跑推理

实际上每个milestone都耗费不少时间。

原以为只要把网络变成代码，基本上这事就完了。但是实际上整个过程最花精力的不是弄懂网络，把它变成代码，而是计算anchor box，各个cell IoU等等步骤。特别是v2引入了一个Anchor Box概念，是个先从样本集取出所有bbox通过knn算出来的值，一开始也没找到太多资料讲清楚，还是靠看代码理解。

截止到春节假期结束，代码半写半抄的，应该是大致弄完了。但是实际上train并没有收敛，用笔记本的1060跑了10个epoch的voc数据集，看tensorboard的lost值，后面9个epoch基本上只是在震荡不再下降，可能有些地方还是不太对。然后长假期结束，这事先告一段落。Megvii的YoloX有说是Anchor free的，貌似不错。。v3成功之后的下一目标打算试试YoloX。
