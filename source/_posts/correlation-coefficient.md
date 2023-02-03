---
title: 相关性系数笔记
date: 2023-02-04 02:29:00
tags: 
  - covariance
  - pearson
---

# 相关性系数笔记

相关性系数可以用于衡量两个随机变量的相关性。包含了以下几个概念

- 方差、协方差、协方差矩阵
- Pearson皮尔逊系数
- Spearman秩相关系数
- Kendall秩相关系数

## 协方差 (covariance)

协方差在概率论和统计学中用于衡量两个变量的总体误差。x和y是两个随机变量，以下函数用于计算x和y序列的协方差。

$cov_{x,y}=\frac{1}{N - 1}\sum_{i=1}^{N}(x_{i}-\bar{x})(y_{i}-\bar{y})$

- $cov_{x,y}$ = covariance between variable x and y
- $x_{i}$ = data value of x
- $y_{i}$ = data value of y
- $\bar{x}$	= mean of x
- $\bar{y}$	= mean of y
- $N$ = number of data values

协方差输出正值表示正相关，负值表示负相关，0表示不相关。值大小不代表相关程度。

## 协方差矩阵 (covariance matrix)

假设有三组协方差$\sigma x$, $\sigma y$, $\sigma z$，则它们组成的协方差矩阵是如下形式：

$\begin{bmatrix}
\sigma x^2 & \sigma x \sigma y & \sigma x \sigma z \\
\sigma y \sigma x & \sigma y^2 & \sigma y \sigma z \\
\sigma z \sigma x & \sigma z \sigma y & \sigma z^2 \\
\end{bmatrix}$

应用：

- 卡尔曼滤波: 状态协方差矩阵P，过程噪声协方差矩阵Q，测量噪声协方差矩阵R
- 金融分析: Monte Carlo方法

## 方差 (variance)

方差是在概率论和统计方差衡量随机变量或一组数据时离散程度的度量。 概率论中方差用来度量随机变量和其数学期望（即均值）之间的偏离程度。 统计中的方差（样本方差）是每个样本值与全体样本值的平均数之差的平方值的平均数。

$var_{x} = \frac{\sum_{i=1}^{N} (x_i - \bar{x}) ^2}{n - 1}$

- $var_{x}$	= sample variance
- $x_i$	= the value of the one observation
- $\bar{x}$	= the mean value of all observations
- $n$ = the number of observations

## 协方差和方差的区别与联系

- 方差是协方差的一种特殊情况，即当两个变量是相同的情况
- **协方差**表示的是**两个变量**的总体的误差，**方差**只表示**一个变量**的误差

## Pearson皮尔逊相关系数

Pearson 相关系数是用协方差除以两个变量的标准差得到的，虽然协方差能反映两个随机变量的相关程度（协方差大于0的时候表示两者正相关，小于0的时候表示两者负相关），但其数值上受量纲的影响很大，不能简单地从协方差的数值大小给出变量相关程度的判断。为了消除这种量纲的影响，于是就有了相关系数的概念。

当两个变量的方差都不为零时，相关系数才有意义，相关系数的取值范围为[-1,1]

$cor_{x,y}=\frac{\sum_{i=1}^{N}(x_{i}-\bar{x})(y_{i}-\bar{y})}{\sqrt{\sum_{1}^{N}(x_i - \bar{x})^2\sum_{1}^{N}(y_i - \bar{y})^2}}$

Python相关

- [scipy.stats.pearsonr](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.pearsonr.html)

应用：

- 金融分析: 继续指数、股票、基金等相关性
- 推荐系统: User-based协同过滤

## Spearman秩相关系数

TODO... (主要解决皮尔逊系数容易受到异常值的的影响的问题)

## Kendall τ秩相关系数

TODO

## Reference

- [统计学习--三种常见的相关系数](https://zhuanlan.zhihu.com/p/34717666)
- [聊聊你知道和不知道的相关性系数](https://cloud.tencent.com/developer/article/1551556#:~:text=%E7%9B%B8%E5%85%B3%E7%B3%BB%E6%95%B0%E6%98%AF%E7%94%A8%E6%9D%A5,%E5%92%8CKendall%20%CF%84%E7%9B%B8%E5%85%B3%E7%B3%BB%E6%95%B0%E3%80%82)
- [【卡尔曼滤波器】2_数学基础_数据融合_协方差矩阵_状态空间方程_观测器问题](https://www.bilibili.com/video/BV12D4y1S7fU/?spm_id_from=333.999.0.0)
- [协方差矩阵有什么意义?](https://www.zhihu.com/question/24283387)
- [如何通俗易懂地理解皮尔逊相关系数？](https://blog.csdn.net/huangfei711/article/details/78456165)