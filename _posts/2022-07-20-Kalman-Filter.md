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
## Base

以一**维运动**为例，假入有一个小车，开始位于$ x = \mu_1 $的位置，但是由于误差的存在，其真实分布是高斯分布[^1]，其方差是$ \sigma_{1}^{2} $，即其原始位置分布是$ N(\mu_1，\sigma_{1}^{2}) $，当该小车经过运动，到达终点位置，但是由于运动也是不准确的（打滑等），其**移动过程**的分布也是高斯分布，移动分布为$ N(\mu_2，\sigma_{2}^{2}) $，那么其最终的位置分布是多少呢？











---
[^1]: Gaussian Distribution，又称正态分布
{% endraw %}
