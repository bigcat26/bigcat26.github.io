---
title: Kalman filter学习笔记
date: 2023-01-20 02:38:00
tags: 
  - control system
  - cybernetic
  - kalman filter
---

# Kalman filter学习笔记

## 前言

本文纯属笔记且未完成，巨坑，不具参考性。

## 应用

- 数据融合
- 目标追踪(SoRT)

## 前置知识点

- [线性代数](https://zhuanlan.zhihu.com/p/362082020)
- [协方差，协方差矩阵](https://www.3wen.net/2023/02/04/correlation-coefficient/)
- [正态分布](https://baike.baidu.com/item/%E6%AD%A3%E6%80%81%E5%88%86%E5%B8%83)

## 运动模型

运动模型可以分为

- 一次运动模型
  - CV: 匀速模型(Constant Velocity)
  - CA: 匀加速模型(Constant Acceleration)
- 二次运动模型
  - CTRV: 恒定转弯率和速度幅度模型(Constant Turn Rate and Velocity)
  - CTRA: 恒定转动率和加速度
  - CSAV: 恒定转向角和速度
  - CCA: 恒定曲率和加速度(Constant Curvature and Acceleration)

## 匀速模型例子

### 系统状态方程

$\hat{x}_k = A\hat{x}_{k - 1} + w_{k - 1}$

- $\hat{x}_k$: 第k次预测值
- $w_{k-1}$: 过程误差协方差矩阵, 符合期望为0，协方差为Q的正态分布 $P(w) \sim \mathcal{N}(0, Q)$
- A: 状态转移矩阵

### 观测/测量方程

$z_{k} = Hx_{k} + v_k$

- $x_{k}$: 第k次观测值
- $v_{k}$: 测量误差协方差矩阵, 符合期望为0，协方差为R的正态分布 $P(v) \sim \mathcal{N}(0, R)$
- H: 测量矩阵

### 计算步骤

#### Predication

$\hat{x}_{\bar{k}} = A\hat{x}_{k - 1}$

$P_{\bar{k}} = AP_{k - 1}A^T + Q$

**P: 状态协方差矩阵(state covariance matrix)**

在使用时协方差矩阵P是一个迭代更新的量，每一轮预测和更新后，P都会更新一个新的值，因此初始化时可以根据估计协定，不用太苛求初始化的精准度，因为随着几轮迭代会越来越趋紧真实值。

#### Measurement update

$K_k = P_{\bar{k}}H^T (HP_{\bar{k}-1}H^T + R)^{-1}$

$\hat{x}_k = \hat{x}_{\bar{k}} + K_k(Z_k - H\hat{x}_{\bar{k}})$

$P_k = (I - K_kH)P_{\bar{k}}$

## 数据融合

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

自己做了一份DR_CAN视频中数据融合excel, [点此下载](/files/dr_can-kalman-filter.xlsx)

## Reference

- [kalman滤波理解三：协方差矩阵的计算](https://blog.csdn.net/u011362822/article/details/95905113)
- [【卡尔曼滤波器】_Kalman Filter_全网最详细数学推导](https://space.bilibili.com/230105574/channel/collectiondetail?sid=6939)
- [CV,CA,CTRV等运动模型，EKF，UKF在运动模型下的分析与实践](https://blog.csdn.net/djfjkj52/article/details/104897314)
- [【SORT算法】系列之深度解读](https://blog.csdn.net/Small_Munich/article/details/112631175)