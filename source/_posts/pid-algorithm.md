---
title: PID算法笔记
date: 2023-01-21 02:00:00
tags: 
  - control system
  - cybernetic
  - pid
---

# PID笔记

参考B站的奇乐编程学院的视频: [通俗易懂的 PID 控制算法讲解](https://www.bilibili.com/video/BV1et4y1i7Gm)

## TODO

- [ ] 串级PID

## 公式

<!-- ![Formula](/img/kalman-filter-formula.png) -->

$u(t)=K_{p} e(t)+K_{i} \int e(t) d t+K_{p} \frac{de}{dt}$

- $u(t)$: PID control variable, 控制输出值
- $K_{p}$: proportional gain, 比例增益
- $e(t)$: error value, t时刻的误差值
- $K_{i}$: integral gain, 积分增益
- ${de}$: change in error value, 误差微分
- ${dt}$: change in time, 时间微分

其中, $K_{p}$, $K_{i}$, $K_{d}$需要调参

## 计算步骤

Python代码

```python

def pid(kp, ki, kd, dt, target, current, prev_integral, prev_error)
    error = target - current
    integral = prev_integral + error * dt
    derivative = (error - prev_error) / dt
    output = kp * error + ki * integral + kd * derivative
    return output, integral, error

error = 0
integral = 0

while True:
    output, integral, error = pid(kp, ki, kd, dt, target, current, integral)
    # device.write(output)
    # delay(dt)
    # current = device.read()

```
