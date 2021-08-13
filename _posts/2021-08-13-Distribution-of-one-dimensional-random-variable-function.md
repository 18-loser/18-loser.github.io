---
layout: post
title: "一维随机变量函数的分布"
data: 2021-08-13 13:46:55 UTC+8
categories: 概率论与数理统计
mathjax: true
---

# 一维随机变量函数的分布



![一维随机变量函数的分布](https://gitee.com/shl1122/pic-bed/raw/master//img/202108131345904.png)



## 1 离散型 ➡ 离散型​

$$
p_{i}=P\left\{X=x_{i}\right\}, Y=g(X), Y \sim\left[\begin{array}{ccc}
g\left(x_{1}\right) & g\left(x_{2}\right) & \cdots \\
p_{1} & p_{2} & \cdots
\end{array}\right]
$$

## 2 连续型 ➡​ 连续性（或混合型）

### 2.1 几何法

$$
\begin{aligned}
&X \sim f_{X}(x), Y=g(x) . \quad F_{Y}(y), f_{Y}(y) \\
&F_{Y}(y) \triangleq p\{Y \leqslant y\}=p\{g(x) \leqslant y\}
\end{aligned}
$$

![image-20210812214855466](https://gitee.com/shl1122/pic-bed/raw/master//img/202108122149975.png)



### 2.2 公式法

$$
f_{Y}(y)= \begin{cases}f_{x}[h(y)] \cdot\left|h^{\prime}(y)\right|, & \alpha<y<\beta \\ 0, & \text { 其他 }\end{cases}
$$

![image-20210812223644084](https://gitee.com/shl1122/pic-bed/raw/master//img/202108122236223.png)

## 3 连续型 ➡ 离散型

若$ X\sim f_X(x) $，且$Y = g(X)$ 是离散的，首先确定 $Y$ 的可能取值 $a$，而后通过计算概率 $ P \lbrace Y=a \rbrace $​，求Y的概率分布

