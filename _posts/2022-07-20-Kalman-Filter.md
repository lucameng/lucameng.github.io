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
  - [Correction](#correction)
- [To sum up](#to-sum-up)

---

## What is Kalman Filter(KF)?

<figure>
	<center>
         <a href="/images/KF/kf1.png"><img src="/images/KF/kf1.png" alt=""></a>
	</center>
</figure>

对于某一个系统，假定知道当前状态$$ X_{t-1} $$，存在以下两个问题：

- 经过时间$$ \Delta t $$后，下一个状态$$ X_{t} $$如何求出？  
- 假定已求出$$ X_{t} $$，同时在$$ t $$时刻接收到传感器的非直接信息$$ Z_{t} $$，如何利用$$ Z_{t} $$来对状态$$ X_{t} $$进行修正呢？

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
$ N(\mu',{\sigma'}^2)=N(\mu_1+\mu_2,\sigma_1^2+\sigma_2^2) \tag{1} \label{eq1}$
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
$ N(\mu',{\sigma'}^2)=N(\mu_1,\sigma_1^2) \times N(\mu_2,\sigma_2^2)=N(\frac{\mu_1\sigma_2^2+\mu_2\sigma_1^2}{\sigma_1^2+\sigma_2^2}，\frac{\sigma_1^2\sigma_2^2}{\sigma_1^2+\sigma_2^2}) \tag{2} \label{eq2}$
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
\pmb{\mu'}=\mu_1+\pmb{K}(\mu_2-\mu_1) \tag{3} \label{eq3}
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

已知小车在$$ t $$时刻的状态$$ \pmb{X_{t}} $$，其表示为一个拥有2个分量的向量，分量分别表示**位置信息**和**速度信息**，即：

<center>
$
\pmb{X_{t}}=\begin{bmatrix}p\\
          v \end{bmatrix}
         =\begin{bmatrix}x_{t}\\
          \dot{x_{t}} \end{bmatrix}
$
</center>

但这个状态不一定准确，其不确定性用**协方差矩阵**表示（[为什么？](https://blog.csdn.net/linxigjs/article/details/93064354)）：

<center>
$
\pmb{P_{t}}=\begin{bmatrix}\Sigma_{pp}&\Sigma_{pv}\\
          \Sigma_{vp}&\Sigma_{vv} \end{bmatrix}
$
</center>

---

### Prediction

将**加速度**等视作外部因素，只考虑小车本身的运动，有：

<center>
$
x_{t}=x_{t-1}+ \dot{x}_{t-1}\Delta t
$
</center>

<center>
$
\dot{x}_{t}=\qquad \quad  \dot{x}_{t-1} \quad
$
</center>

用矩阵表示：

<center>
$
\pmb{\hat{X}_{t}}=\begin{bmatrix} 1 &\Delta t\\
          0 & 1 \end{bmatrix} \pmb{\hat{X}_{t-1}} 
          =\pmb{F} \pmb{\hat{X}_{t-1}}
$
</center>

> 注：考虑$$ \Delta t $$为常量，此时状态转换矩阵$$ F_t $$不变，故简写为$$ F $$，下同。

在**状态变化**的过程中引入了新的**不确定性**，根据协方差的乘积公式：

<center>
$
Cov(x)=\Sigma
$
</center>

<center>
$
Cov(\pmb{A}x)=\pmb{A}\Sigma\pmb{A}^T \tag{4} \label{eq4}
$
</center>

可得：

<center>
$
\pmb{\hat{X}_{t}} = \pmb{F} \pmb{\hat{X}_{t-1}}
$
</center>

<center>
$
\pmb{P_{t}} = \pmb{F} \pmb{P_{t-1}}\pmb{F}^T
$
</center>

---

考虑外部因素$$ \pmb{u_{t-1}} $$，如引入**加速度** $$a_{t-1} = \ddot{x}_{t-1}$$:

<center>
$
x_{t}=x_{t-1}+ \dot{x}_{t-1}\Delta t + \frac{1}{2} \ddot{x_{t-1}} \Delta t^2
$
</center>

<center>
$
\dot{x}_{t}=\qquad \quad \dot{x}_{t-1} \quad+\quad \ddot{x_{t-1}} \Delta t
$
</center>

写成矩阵形式：

<center>
$
\pmb{\hat{X}_{t}} = \pmb{F} \pmb{\hat{X}_{t-1}} + \begin{bmatrix} \frac{1}{2} \Delta t^2\\
          \Delta t \end{bmatrix} \ddot{x}_{t-1} 
$
</center>

<center>
$
= \pmb{F} \pmb{\hat{X}_{t-1}} + \pmb{B}\pmb{u_{t-1}}
$
</center>

同时，还存在环境误差，以$$ \pmb{Q} $$表示。最终预测的形式呈现为：

<center>
$
\pmb{\hat{X}_{t}} = \pmb{F} \pmb{\hat{X}_{t-1}} + \pmb{B} \ddot{x_{t-1}} 
$
</center>

<center>
$
\pmb{P_{t}} = \pmb{F} \pmb{P_{t-1}}\pmb{F}^T + \pmb{Q}
$
</center>

---
小结：

- **新的最优估计**是**之前最优估计**的**预测**加上已知的**外界影响**的修正。  
- **新的不确定度**是**预测的不确定度**加上**环境的不确定度**。  


---

### Correction

通过**预测过程**我们已经得到了$$ \pmb{X_{t}} $$和$$ \pmb{P_{t}} $$。下一步我们要通过**观测**到的测量值$$ \pmb{Z_{t}} $$对$$ \pmb{X_{t}} $$和$$ \pmb{P_{t}} $$进行**修正更新**。由于这个过程不再涉及$$ t-1 $$时刻的状态和误差，因此将$$ \pmb{X_{t}} $$、$$ \pmb{P_{t}} $$和$$ \pmb{Z_{t}} $$省略为$$ \pmb{X} $$、$$ \pmb{P} $$和$$ \pmb{Z} $$，以方便阅读。

由于传感器观测得到的数据信息$$ \pmb{Z} $$与$$ \pmb{X} $$的尺度不尽相同，因此我们需要一个转换矩阵$$ \pmb{H} $$将$$ \pmb{X} $$转换为$$ \pmb{Z} $$的尺度，即：

<center>
$
\pmb{\hat{Z}} = \pmb{H} \pmb{\hat{X}}  
$
</center>

借助公式$\eqref{eq4}$，有：

<center>
$
\pmb{\Sigma} = \pmb{H} \pmb{P}  \pmb{H}^T 
$
</center>

目前，我们得到了$$ \vec{x} $$方向（一维）上的两个分布：

- **预测（Prediction）**值$$ \pmb{X} $$的分布$$ N(\vec{x};\mu_1,\sigma_1^2) $$  
- **测量（Measurement）**值$$ \pmb{Z} $$的分布$$ N(\vec{x};\mu_2,\sigma_2^2) $$


<figure>
	<center>
         <a href="/images/KF/kf2.png"><img src="/images/KF/kf2.png" alt=""></a>
	</center>
</figure>

在修正过程中，这两个分布融合得到$$ \pmb{X'} $$，记其分布为$$ N_{fused} $$，由公式$\eqref{eq2}$得到：

<center>
$
N_{fused}(\vec{x};\mu_{fused},\sigma_{fused}^2) = N(\vec{x};\mu_1,\sigma_1^2) \times N(\vec{x};\mu_2,\sigma_2^2)
$
</center>

扩展到二维情况，由公式$\eqref{eq4}$，我们得到：

- **预测（Prediction）**值$$ \pmb{X} $$对应的均值和协方差为$$ (\mu_1,\Sigma_1)=(\pmb{H \hat{X}},\pmb{HPH^T}) $$  
- **测量（Measurement）**值$$ \pmb{Z} $$对应的均值和协方差为$$ (\mu_2,\Sigma_2)=(\pmb{Z},\pmb{R}) $$ 

借助公式$\eqref{eq3}$，有：

<center>
$
\pmb{HX'} = \pmb{HX} + \pmb{K} (Z-\pmb{HX}) 
$
</center>

<center>
$
\pmb{HP'H^T} = \pmb{HPH^T} - \pmb{K}\pmb{HPH^T} 
$
</center>

其中，

<center>
$
\pmb{K} = \pmb{HPH^T(HPT^T+R)^{-1}}
$
</center>

将第二个式子左乘$$ H^{-1} $$；第三个式子左乘$$ H^{-1} $$，右乘$$ (H^T)^{-1} $$，得到：

<center>
$
\pmb{X'} = \pmb{X} + \pmb{K'} (\pmb{Z}-\pmb{HX}) 
$
</center>

<center>
$
\pmb{P'} = \pmb{P} - \pmb{K'}\pmb{HP} 
$
</center>

其中，

<center>
$
\pmb{K'} = \pmb{PH^T(HPT^T+R)^{-1}}
$
</center>

---
## To sum up

整个过程中，卡尔曼滤波共有5个公式，分别为**预测过程**的2个公式和**测量修正过程**的3个公式：

- 预测过程：

<center>
$
\pmb{\hat{X}_{t}} = \pmb{F} \pmb{\hat{X}_{t-1}} + \pmb{B} \pmb{u_{t-1}}
$     
</center>

<center>
$
\pmb{P_{t}} = \pmb{F} \pmb{P_{t-1}}\pmb{F}^T + \pmb{Q_{t-1}}
$
</center>

- 更新过程：


<center>
$
\pmb{K'} = \pmb{PH^T(HPT^T+R)^{-1}}
$
</center>

<center>
$
\pmb{X'} = \pmb{X} + \pmb{K'} (\pmb{Z}-\pmb{HX}) 
$
</center>

<center>
$
\pmb{P'} = \pmb{P} - \pmb{K'}\pmb{HP} 
$
</center>

---
[^1]: Gaussian Distribution，又称正态分布


{% endraw %}
