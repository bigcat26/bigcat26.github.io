---
title: GPU cloud computing market
date: 2022-11-12 12:00:00
tags: 
  - ai
  - cloud
---

# GPU云计算市场

这两天正式开始用[AutoDL](https://www.autodl.com/register?code=9b0db90f-2ad8-4a59-840c-dcc0765ba3ed)炼丹。感觉还是比较容易上手的。

![Dashboard](/img/autodl_dashboard_1.png)

![Tensorboard](/img/autodl_tensorboard.png)

忘了怎么找到这个AutoDL的，11-11前有重新装机的打算，准备装一个GPU搞DL训练。但是好几部分原因，搁置了装机计划，改为线上租赁。

现在用的是一个￥0.6/h的1080ti实例跑一下facenet的训练，作为前期的学习和验证足够了。它最低貌似能到0.49/h (TITAN)，但不太好抢。这个价格也就每小时一度电左右的费用，自己装机除了硬件费用还要给电费，而且硬件还有贬值，还要自己搭环境，收集数据，没有直接用线上的来的方便。

## 表格

随便简单采样了一些，发现这个领域还是比较卷的。

| 算力市场                                          | 数据集               | 备注                                  |
| ------------------------------------------------- | -------------------- | ------------------------------------- |
| [AutoDL](https://www.autodl.com/register?code=9b0db90f-2ad8-4a59-840c-dcc0765ba3ed)                 | 共享/百度云/阿里云盘 |                                       |
| [featurize](https://featurize.cn/)                | 共享                 | 可用较少, 没价格优势                  |
| [智星云](http://www.ai-galaxy.cn/)                | 公共                 |                                       |
| [矩池云](https://matpool.com/host-market/gpu)     |                      | 1.00/h 起                             |
| [恒源云](https://gpushare.com/)                   | 公共/共享            | 0.60/h 起, 类似autodl, 注册才能看价格 |
| [腾讯云](https://buy.cloud.tencent.com/price/gpu) |                      | 没仔细看                              |
