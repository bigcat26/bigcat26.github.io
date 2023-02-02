---
title: 梯度下降拟合曲线
date: 2023-02-01 00:00:08
tags: 
  - gradient descent
  - least square method

---

# 梯度下降拟合曲线

原本打算使用python代码实现梯度下降曲线拟合。搜索了一圈没有找到讲得非常详细的文章。自己实践了一下记录在此。

注：公式是后期整理的没有很仔细，也许会有些细节上的问题，仅供参考。

## 原理

首先确定拟合曲线的函数是怎样的，这里用一元k次方程：

$y(w_0,w_1...w_k) = w_0 + w_1 x + w_2 x^2 ... + w_k x^k = \sum_{n=0}^{k} {w_n}{x^n}$

然后，确定使用最小二乘损失函数：

$L = \sum_{i=0}^{j} \frac{1}{2j} (\hat{y_i} - 𝑦_i) ^ 2$

这里的$\hat{y}$是观测值, $y_i$是理论值(ground truth)，$j$是一次迭代的样本个数。

梯度下降优化，就是对loss函数的每一个w权重分量分别计算梯度，然后每次迭代更新一次梯度。

对于每个w权重的梯度，也就是每个$w_g$的偏导数，计算公式是：$\frac{d}{dw_g}L = \frac{d}{dw_g}\sum_{i=0}^{j} \frac{1}{2j} (\hat{y_i} - 𝑦_i) ^ 2$

这里以$k = 2$, g为1和2的情况为例展开推导，首先算出$(\hat{y} - y)^2 = $

$( w_0^2 + w_0w_1x + w0w_2^2 - w_0y$

$+ w_0w_1x + w_1^2x^2 + w_1w_2x^3 - w_1xy$

$+ w_0w_2x^2 + w_1w_2x^3 + w_2^2x^4 - w_2x^2y$

$- w_0y - w_1xy - w_2x^2y + y^2)$

对$w_1$求导后, 得: 

$= (w_0x + w_0x + 2w_1x^2 + w_2x^3 - xy + w_2x^3 - xy)$

$= (2w_0x + 2w_1x^2 + 2w_2x^3 - 2xy)$

$= 2x(w_0 + w_1x + w_2x^2 - y)$

对$w_2$求导后, 得: 

$= 2x^2(w_0 + w_1x + w_2x^2 - y)$

所以 $\frac{d}{dw_g}L = x^g(\sum_{n=0}^{k} {w_n}{x^n} - y)$

对于每次迭代权重$w_k$的梯度为:

$gradient(w_g) = \sum_{i=1}^{j} \frac{1}{j} x^g(\sum_{n=0}^{k} {w_n}{x^n} - y)$

下面就是权重更新的公式：

$w_k = w_k - \epsilon \frac{dL}{d w_g}$

其中 $\epsilon$ 为学习率。

## 测试数据

这里设要拟合一个接近一元三次函数$f(x) = ax^3+bx^2+cx+d$分布的散点的曲线，生成样本数据。

```python
import math
import numpy as np
import matplotlib.pyplot as plt

# -3x^3 + 2x^2 + -4x + 2
ground_truth_w = [-3, 2, -4, 2]
ground_truth_x = np.arange(-5, 4, 0.5)
ground_truth_y = np.polyval(ground_truth_w, ground_truth_x)
# 加噪音
ground_truth_y_noise = [y + np.random.randn() * 30 for y in ground_truth_y]
```

![Sample](/img/gd_sample.png)

以上散点是要拟合的曲线的输入样本。

## 拟合代码

```python
import math
import numpy as np
import matplotlib.pyplot as plt

# 没做SGD, 每次使用所有样本计算梯度
batch_size = len(ground_truth_y)
# 学习率
eps = [0.0001, 0.0001, 0.0001]
# 随机初始化系数权重, 权重个数-1即为N次方的N
# weights = [np.random.randn(4), np.random.randn(3), np.random.randn(2)]
weights = [np.zeros(4), np.zeros(3), np.zeros(2)]
# 迭代次数
epochs = [2000, 2000, 2000]

def evaluate_loss(x, y, weights):
    s = 0
    bs = len(y)
    for i in range(bs):
        s = s + (np.polyval(weights, x[i]) - y[i]) ** 2
    return s / bs;

def evaluate_gradient(x, y, weights, epsilon):
    power = len(weights)
    for g in range(len(weights)):
        gradient = 0
        for i in range(batch_size):
            gradient += x[i] ** (power - 1 - g) * (np.polyval(weights, x[i]) - y[i]) / batch_size
        weights[g] = weights[g] - epsilon * gradient
        
def fit_curve(x, y, weights, epochs, epsilon):
    for i in range(epochs):
        evaluate_gradient(x, y, weights, epsilon)
        if i % 100 == 0:
            loss = evaluate_loss(x, y, weights)
            print(f'epoch:{i:8} loss:{loss:.4f}')

for i in range(len(weights)):
    fit_curve(ground_truth_x, ground_truth_y_noise, weights[i], epochs[i], eps[i])

```

输出图像：

![Result](/img/gd_curvefit.png)

## 常见问题

### 训练出现nan

学习率太大, 样本数据y值太大

### loss降不下来

loss下降很小说明优化空间已经不大了, 不会降到1以下的

### 拟合不收敛

可能之一是gradient算错了，比如`x[i]`的指数算错，或者weights的顺序没对应上之类。
