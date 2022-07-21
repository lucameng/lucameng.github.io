---
layout: post
title: Introduction to Kalman Filter
description: "深入浅出学习卡尔曼滤波"
tags: [自动驾驶]
image:
  feature: abstract-11.jpg
modified: 2022-07-20
---


{% raw %}
## As an example

以**一维运动**为例，假入有一个小车，开始位于$$ x=\mu_1 $$的位置，但是由于误差的存在，其真实分布是高斯分布[^1]，其方差是$$ \sigma_{1}^{2} $$，即其原始位置分布是$$ N(\mu_1,\sigma_{1}^{2}) $$，当该小车经过运动，到达终点位置，但是由于运动也是不准确的（打滑等），其**移动过程**的分布也是高斯分布，移动分布为$$ N(\mu_2,\sigma_{2}^{2}) $$，那么其最终的位置分布是多少呢？

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

## One Steps Further

下面将**感知过程**中的利用到的贝叶斯法则扩展到**多维**，这也是卡尔曼滤波的**核心**。

已知在一维模式下，两个分布融合得到的均值和方差分别为：

<center>
$
\mu'=\frac{\mu_1\sigma_2^2+\mu_2\sigma_1^2}{\sigma_1^2+\sigma_2^2}=\mu_1+\frac{\sigma_1^2(\mu_2-\mu_1)}{\sigma_1^2+\sigma_2^2}

{\sigma'}^2=\frac{\sigma_1^2\sigma_2^2}{\sigma_1^2+\sigma_2^2}=\sigma_1^2-\frac{\sigma_1^4}{\sigma_1^2+\sigma_2^2} 
$


</center>

---
[^1]: Gaussian Distribution，又称正态分布


{% endraw %}
