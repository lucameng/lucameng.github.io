---
layout: post
title: Introduction to Kalman Filter
description: "深入理解卡尔曼滤波"
tags: [自动驾驶]
image:
  feature: abstract-11.jpg
modified: 2022-07-20
---


{% raw %}
**Table of Contents**
- [What is Kalman Filter(KF)?](#what-is-kalman-filterkf)
- [As an example](#as-an-example)
- [One steps further](#one-steps-further)
- [Multi-dimention](#multi-dimention)
  - [Prediction](#prediction)

---

## What is Kalman Filter(KF)?

<figure>
	<center>
         <a href="/images/KF/kf1.png"><img src="/images/KF/kf1.png" alt=""></a>
	</center>
</figure>

对于某一个系统，假定知道当前状态$$ X_t $$，存在以下两个问题：

- 经过时间$$ \Delta t $$后，下一个状态$$ X_{t+1} $$如何求出？  
- 假定已求出$$ X_{t+1} $$，同时在$$ t+1 $$时刻接收到传感器的非直接信息$$ Z_{t+1} $$，如何利用$$ Z_{t+1} $$来对状态$$ X_{t+1} $$进行修正呢？

以上两个问题就是卡尔曼滤波要解决的，可以简要概括如下：

- 预测未来  
- 修正当下

---

## As an example

以**一维运动**为例，假定有一个小车，开始位于$$ x=\mu_1 $$的位置，但是由于误差的存在，其真实分布是高斯分布[^1]，其方差是$$ \sigma_{1}^{2} $$，即其原始位置分布是$$ N(\mu_1,\sigma_{1}^{2}) $$，当该小车经过运动，到达终点位置，但是由于运动也是不准确的（受轮胎打滑等因素影响），其**移动过程**的分布也是高斯分布，移动分布为$$ N(\mu_2,\sigma_{2}^{2}) $$，那么其最终的位置分布是怎样的呢？

<figure>
	<center>
         <a href="/images/KF/eg1.png"><img src="/images/KF/eg1.png" width="400px" alt=""></a>
	</center>
</figure>

这是一个**预测过程**，求预测位置符合**全概率法则**，即：

<center>
$ N(\mu',{\sigma'}^2)=N(\mu_1+\mu_2,\sigma_1^2+\sigma_2^2) $
</center>

---

考虑另外一种情况，同样是**一维运动**，有一个小车开始时位于$$ x=\mu_1 $$的位置，但由于误差的存在，其真实分布是方差为$$ \sigma_1^2 $$的高斯分布，即其开始位置的分布为$$ N(\mu_1,\sigma_{1}^{2}) $$，此时有一个**传感器**检测到该小车位于$$ x=\mu_2 $$，分布的方差为$$ \sigma_2^2 $$，那么小车的真实分布是多少呢？

<figure>
	<center>
         <a href="/images/KF/eg2.png"><img src="/images/KF/eg2.png" width="400px" alt=""></a>
	</center>
</figure>


这是一个**感知过程**，其符合**贝叶斯法则**，最终分布是两个分布相乘，即：

<center>
$ N(\mu',{\sigma'}^2)=N(\mu',\sigma_1^2) \times N(\mu',\sigma_2^2)=N(\frac{\mu_1\sigma_2^2+\mu_2\sigma_1^2}{\sigma_1^2+\sigma_2^2}，\frac{\sigma_1^2\sigma_2^2}{\sigma_1^2+\sigma_2^2}) $
</center>

---

## One steps further

下面将**感知过程**中的利用到的贝叶斯法则扩展到**多维**，这也是卡尔曼滤波的**核心**。

已知在一维模式下，两个分布融合得到的均值和方差分别为：

<center>
$
\mu'=\frac{\mu_1\sigma_2^2+\mu_2\sigma_1^2}{\sigma_1^2+\sigma_2^2}=\mu_1+\frac{\sigma_1^2(\mu_2-\mu_1)}{\sigma_1^2+\sigma_2^2}
$
</center>

<center>
$
{\sigma'}^2=\frac{\sigma_1^2\sigma_2^2}{\sigma_1^2+\sigma_2^2}=\sigma_1^2-\frac{\sigma_1^4}{\sigma_1^2+\sigma_2^2} 
$
</center>

我们令

<center>
$
K=\frac{\sigma_1^2}{\sigma_1^2+\sigma_2^2}
$
</center>

则有：

<center>
$
\mu'=\mu_1+K(\mu_2-\mu_1)
$
</center>

<center>
$
{\sigma'}^2=\sigma_1^2-K\sigma_1^2
$
</center>

将该结果扩展到多维情况下，得到：

<center>
$
\pmb{K}=\Sigma_1(\Sigma_1+\Sigma_2)^{-1}
$
</center>

<center>
$
\pmb{\mu'}=\mu_1+\pmb{K}(\mu_2-\mu_1) 
$
</center>

<center>
$
\Sigma'=\Sigma_1-\pmb{K}\Sigma_1
$
</center>

该公式会在卡尔曼滤波的**修正更新过程**中用到。

---

## Multi-dimention

考虑一个**二维状态**的例子：

已知小车当前状态$$ \pmb{X_t} $$，其表示为一个拥有2个分量的向量，分量分别表示**位置信息**和**速度信息**，即：

<center>
$
\pmb{X_t}=\begin{bmatrix}p\\
          v \end{bmatrix}
         =\begin{bmatrix}x_t\\
          \dot{x_t} \end{bmatrix}
$
</center>

但这个状态不一定准确，其不确定性用**协方差矩阵**表示（[为什么？](https://blog.csdn.net/linxigjs/article/details/93064354)）：

<center>
$
\pmb{P_t}=\begin{bmatrix}\Sigma_{pp}&\Sigma_{pv}\\
          \Sigma_{vp}&\Sigma_{vv} \end{bmatrix}
$
</center>

---

### Prediction

将**加速度**等视作外部因素，只考虑小车本身的运动，有：

<center>
$
x_{t+1}=x_{t}+ \dot{x}_{t}\Delta t
$
</center>

<center>
$
\dot{x}_{t+1}=\qquad \dot{x}_{t} \quad
$
</center>

用矩阵表示：

<center>
$
\pmb{\hat{X}_{t+1}}=\begin{bmatrix} 1 &\Delta t\\
          0 & 1 \end{bmatrix} \pmb{\hat{X}_{t}} 
          =\pmb{F} \pmb{\hat{X}_{t}}
$
</center>

在**状态变化**的过程中引入了新的**不确定性**，根据协方差的乘积公式：

<center>
$
Cov(x)=\Sigma
$
</center>

<center>
$
Cov(\pmb{A}x)=\pmb{A}\Sigma\pmb{A}^T
$
</center>

可得：

<center>
$
\pmb{\hat{X}_{t+1}} = \pmb{F} \pmb{\hat{X}_{t}}
$
</center>

<center>
$
\pmb{P_{t+1}} = \pmb{F} \pmb{P_{t}}\pmb{F}^T
$
</center>

---

考虑外部因素$$ \pmb{u_t} $$，如引入**加速度** $$a_t = \ddot{x_t}$$:

<center>
$
x_{t+1}=x_{t}+ \dot{x}_{t}\Delta t + \frac{1}{2} \ddot{x_t} \Delta t^2
$
</center>

<center>
$
\dot{x}_{t+1}=\qquad \dot{x}_{t} +\qquad \ddot{x_t} \Delta t
$
</center>

写成矩阵形式：

<center>
$
\pmb{\hat{X}_{t+1}} = \pmb{F} \pmb{\hat{X}_{t}} + \begin{bmatrix} \frac{1}{2} \Delta t^2\\
          \Delta t \end{bmatrix} \ddot{x_t} 
$
</center>

<center>
$
\quad = \pmb{F} \pmb{\hat{X}_{t}} + \pmb{B}\pmb{u_t}
$
</center>

同时，还存在环境误差，以$$ \pmb{Q_t} $$表示。最终预测的形式呈现为：

<center>
$
\pmb{\hat{X}_{t+1}} = \pmb{F} \pmb{\hat{X}_{t}} + \pmb{B} \ddot{x_t} 
$
</center>

<center>
$
\pmb{P_{t+1}} = \pmb{F} \pmb{P_{t}}\pmb{F}^T + \pmb{Q_t}
$
</center>




---
[^1]: Gaussian Distribution，又称正态分布


{% endraw %}
