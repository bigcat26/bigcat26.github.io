---
title: Kalman filter笔记1
date: 2023-01-20 02:38:00
tags: 
  - control system
  - cybernetic
  - kalman filter
---

# Kalman filter笔记(未完成)

参考B站B站的DR.CAN的视频: [【卡尔曼滤波器】_Kalman Filter_全网最详细数学推导](https://space.bilibili.com/230105574/channel/collectiondetail?sid=6939)

## 前言

本文纯属笔记，且未完成，不具参考性。

## 概念

### 协方差 covariance

$cov_{x,y}=\frac{\sum(x_{i}-\bar{x})(y_{i}-\bar{y})}{N-1}$

- $cov_{x,y}$	=	covariance between variable x and y
- $x_{i}$	=	data value of x
- $y_{i}$	=	data value of y
- $\bar{x}$	=	mean of x
- $\bar{y}$	=	mean of y
- $N$	=	number of data values

协方差在概率论和统计学中用于衡量两个变量的总体误差。**而方差是协方差的一种特殊情况，即当两个变量是相同的情况**。协方差表示的是两个变量的总体的误差，这与只表示一个变量误差的方差不同。

### 方差 variance

$S^{2} = \frac{\sum (x_i - \bar{x}) ^2}{n - 1}$

- $S^2$	=	sample variance
- $x_i$	=	the value of the one observation
- $\bar{x}$	=	the mean value of all observations
- $n$	=	the number of observations

方差是在概率论和统计方差衡量随机变量或一组数据时离散程度的度量。 概率论中方差用来度量随机变量和其数学期望（即均值）之间的偏离程度。 统计中的方差（样本方差）是每个样本值与全体样本值的平均数之差的平方值的平均数。

## 公式

![Formula](/img/kalman-filter-formula.png)

$\hat{X}_{k}=\hat{X}_{k-1} + K_k * (Z_k - \hat{X}_{k-1})$

- $\hat{X}$: 当前估计值
- $\hat{X}_{k-1}$: 前一次估计值
- $Z_k$: 当前测量值
- $K_k$: Kalman Gain,卡尔曼增益/卡尔曼系数

$K_k = \frac{e_{EST_{k-1}}}{e_{EST_{k-1}} + e_{MEA_k}}$

其中:

- ${e}_{EST}$: Error estimate, 估计误差
- ${e}_{MEA}$: Error measurement, 测量误差

${e}_{MEA}$一般由测量工具得出（传感器等）

当K=0时，${e}_{EST}$和$\hat{X}$可以为任何值，它将会在K=1时更新。

在K时刻

- 当${e}_{EST} \gg {e}_{MEA}$, 则 $K_k \to 1$, $X_k = Z_k$
- 当${e}_{EST} \ll {e}_{MEA}$, 则 $K_k \to 0$, $X_k = X_{k-1}$

## 计算步骤

![Calculation](/img/kalman-filter-cal-step.png)

1. K=0初始化${e}_{EST}$, $\hat{X}$, ${Z}_{k}$, ${e}_{MEA}$可忽略
2. K = K + 1
3. 根据${e}_{MEA}$和${e}_{EST_{k-1}}$计算${K}_{k}$
4. 从${e}_{MEA}$和${Z}_{k}$计算$\hat{k}$
5. 更新${e}_{EST_{k}}$
6. 重复2

## Python代码

这里假设e_measurement不跟随k而变化

```python
import numpy as np

def kalman(xhat_prev, e_estimate_prev, e_measurement, zk):
    kalman_gain = e_estimate_prev / (e_estimate_prev + e_measurement)
    xhat = xhat_prev + kalman_gain * (zk - xhat_prev)
    e_estimate = (1 - kalman_gain) * e_estimate_prev
    return xhat, e_estimate

def kalman_filter(zk: np.array, e_measurement, e_estimate0):
    e_estimate = e_estimate0
    r = np.ndarray(len(zk) + 1, np.float32)
    r[0] = 0
    for i in range(0, len(zk)):
        r[i + 1], e_estimate = kalman(r[i], e_estimate, e_measurement, zk[i])
    return r
```

## Excel

仿造视频中demo的Excel做了一份, [点此下载](/files/dr_can-kalman-filter.xlsx)