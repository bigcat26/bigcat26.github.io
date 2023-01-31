---
title: 梯度下降拟合曲线
date: 2023-02-01 00:00:08
tags: 
  - gradient descent
  - least square method

---

# 梯度下降拟合曲线

原本打算使用python代码实现多元多次梯度下降。搜索了一圈没有找到讲得非常详细的文章。自己实践了一下记录在此。

注：公式是后期整理的没有很仔细，也许会有些细节上的问题，仅供参考。

## 原理

首先确定拟合曲线的函数是怎样的，这里用N元N次方程：

$y(w_0,w_1...w_j) = w_0 + w_1 x + w_2 x^2 ... + w_j x^j $

然后，确定使用最小二乘损失函数：

$L = \sum_{i=0}^{j} \frac{1}{2} (\hat{y_i} - 𝑦_i) ^ 2$

这里的$\hat{y}$是观测值, $y_i$是理论值(ground truth)，$j$是一次迭代的样本个数。

梯度下降优化，就是对loss函数的每一个w权重分量分别计算梯度，然后每次迭代更新一次梯度。

对于每个w权重的梯度，也就是每个w的偏导数，计算公式是：$\frac{dL}{dw_k}$

这里可以使用链式法则，设$a = \frac{1}{2} (\hat{y} - 𝑦) ^ 2$

则 $\frac{dL}{dw_k}$ = $\frac{dL}{da} * \frac{da}{dw_k}$

$\frac{dL}{da} = 2a$

$\frac{da}{dw_k} = \frac{d}{dw_k} \frac{1}{2} (\sum_{j=0}^{n} w_j x^j - y) = x^k (\sum_{i=0}^{j} w_ix^i - y)$

这里以$k = 2$为例展开推导:

$\frac{da}{dw_2} = \frac{1}{2} (2w_0x^2 + 2w_1x^3 +2w_2x^4 - 2x^2y) = x^2(w_0 + w_1x + w_2x^2 - y)$

最终得$\frac{dL}{dw_k} = 2x^k (\sum_{i=0}^{j} w_ix^i - y_i)$

因此对于每次batch size为$b$的迭代权重$w_k$的梯度为:

$gradient(w_k) = \sum_{i=1}^{b}  x_i^k (\sum_{j=0}^{n} w_j x_i^j - y_i)$

下面就是权重更新的公式：

$w_k = w_k - \epsilon \frac{dL}{d w_k}$

其中 $\epsilon$ 为学习率。

## 测试数据

这里设要拟合一个接近三元三次函数$f(x) = ax^3+bx^2+cx+d$分布的散点的曲线，生成样本数据。

```python
import math
import numpy as np
import matplotlib.pyplot as plt

# 假设曲线方程 2x^3 + 4x^2 + 7x + 1
ground_truth_coeff = [2, 4, 7, 2]

ground_truth_x = np.linspace(-10, 10, 21)
ground_truth_y = np.polyval(ground_truth_coeff, ground_truth_x)
# 加噪音
ground_truth_y_noise = [y + np.random.randn() * 100 for y in ground_truth_y]

plt.plot(ground_truth_x, ground_truth_y, label='function curve')
plt.scatter(ground_truth_x, y=ground_truth_y_noise, label='sample with noise')
plt.legend()
plt.show()
```

![Sample](/img/gd-sample.png)

以上散点是要拟合的曲线的输入样本。

## 拟合代码

```python
import math
import numpy as np
import matplotlib.pyplot as plt

# 学习率
eps = 0.0000001
# 迭代次数
epochs = 10000
# 没做SGD, 每次使用所有样本计算梯度
batch_size = len(ground_truth_y)
# 随机初始化系数权重
weights = np.random.randn(4)

def evaluate_loss(weights, x, y):
    s = 0
    bs = len(x)
    for i in range(bs):
        s = s + (np.polyval(weights, x[i]) - y[i]) ** 2
    return s / bs;

def evaluate_gradient(x, y, weights):
    power = len(weights)
    for g in range(len(weights)):
        p = 0
        gradient = 0
        for i in range(batch_size):
            gradient += (x[i] ** (power - 1 - g)) * (np.polyval(weights, x[i]) - y[i])
        weights[g] = weights[g] - eps * gradient
    
for i in range(epochs):
    evaluate_gradient(ground_truth_x, ground_truth_y_noise, weights)
    if i % 1000 == 0:
        loss = evaluate_loss(weights, ground_truth_x, ground_truth_y_noise)
        print(f'epoch:{i} loss:{loss}')
        
# 画图
result_y = np.polyval(weights, ground_truth_x)

plt.scatter(ground_truth_x, ground_truth_y_noise, label='sample')
plt.plot(ground_truth_x, result_y, label='result')
plt.legend()
plt.show()
```

输出结果:

```text
epoch:0 loss:224137.58528031068
epoch:1000 loss:7817.904451520894
epoch:2000 loss:7816.040577617981
epoch:3000 loss:7815.629924575401
epoch:4000 loss:7815.225656784621
epoch:5000 loss:7814.82757311734
epoch:6000 loss:7814.435531941864
epoch:7000 loss:7814.049395009907
epoch:8000 loss:7813.669027372575
epoch:9000 loss:7813.294297301407
```

可以看到其实在第1000个epoch, loss就不怎么降了, 还是强行跑了10k个。

最终输出的权重：array([2.09759084, 4.49785889, 1.57035344, 1.76617769])

图像：

![Result](/img/gd-curvefit.png)
