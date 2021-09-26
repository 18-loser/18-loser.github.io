---
layout: post
title: "伽马函数"
data: 2021-08-17 22:49:54 UTC+8
categories: 概率论与数理统计
mathjax: true
---

# 数字特征

## 数学期望

数学期望就是随机变量的取值与取值概率乘积的和.

$$
X \sim p_{i} \Rightarrow E X=\sum x_{i} p_{i} \tag1
$$

$$
X \sim f(x) \Rightarrow E X=\int_{-\infty}^{+\infty} x f(x) \mathrm{d} x \tag2
$$

上述 $(1)$ $(2)$ 两个公式就是数学期望**离散型**和**连续型**数学公式.

## 伽马函数

伽马函数的定义式 ${\Gamma(\alpha)=\int_0^{+\infty}x^{a-1}e^{-x}dx (a>0)}$,往下推到可得到 ${ (1) }$

$$
\Gamma(\alpha)=\left\{\begin{array}{l}\int_{0}^{+\infty} x^{\alpha-1} e^{-x} d x \\ \\ 2 \int_{0}^{+\infty} x^{2 \alpha-1} e^{-x^{2}} d x\end{array}\right.\tag1
$$

$$
(2) \Gamma(\alpha+1)=\alpha\Gamma(\alpha), \alpha>0 \tag2
$$

由 ${(1)}$ ${(2)}$ 得出来下面的公式

$$
\Gamma(n+1)=n !
$$

$$
\Gamma(1)=1, \quad \Gamma\left(\frac{1}{2}\right)=\sqrt{\pi}
$$

