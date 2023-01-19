---
title: Kalman filter笔记
date: 2023-01-20 02:38:00
tags: 
  - control system
  - cybernetic
---

# Kalman filter笔记

来源B站的DR.CAN的视频

## 公式

![Formula](/img/kalman-filter-formula.png)

递归思想 (动态规划)

`$\hat{X}_{k}=\hat{X}_{k-1} + K_k * (Z_k - \hat{X}_{k-1})$`

*   `$\hat{X}$`: 当前估计值
*   `$\hat{X}_{k-1}$`: 前一次估计值
*   `$Z_k$`: 当前测量值
*   `$K_k$`: Kalman Gain,卡尔曼增益/卡尔曼系数

参数:

*   估计误差: `${e}_{EST}$`, Error estimate
*   测量误差: `${e}_{MEA}$`, Error measurement

`$K_k = \frac{e_{EST_{k-1}}}{e_{EST_{k-1}} + e_{MEA_k}}$`

`${e}_{MEA}$`作为先验值，一般由测量工具得出（传感器等）

当K=0时，`${e}_{EST}$`和`$\hat{X}$`可以为任何值，它将会在K=1时更新。

## 讨论

在K时刻，如果

- `E(est) >> E(mea)`, 则 Kk -> 1, X(k) = Z(k)
- `E(est) << E(mea)`, 则 Kk -> 0, X(k) = X(k-1)

## 计算步骤

![Calculation](/img/kalman-filter-cal-step.png)

1.   K=0初始化`${e}_{EST}$`, `$\hat{X}$`, `${Z}_{k}$`, `${e}_{MEA}$`可忽略
2.   `K = K + 1`
3.   根据`${e}_{MEA}$`和`${e}_{EST_{k-1}}$`计算`${K}_{k}$`
4.   从`${e}_{MEA}$`和`${Z}_{k}$`计算`$\hat{k}$`
5.   更新`${e}_{EST_{k}}$`
6.   重复2
