---
layout: post
title: codeforces718C Sasha and Array
date: 2018-09-16 17:40:08 +0800
categories: training
tags: 矩阵快速幂 线段树及可持久化
img: https://vexoben.github.io/assets/images/Blog2/2018-10-06-codeforces718c-sasha-and-array.png
---

题目链接: [codeforces718C Sasha and Array][1]

(刷新以获取数学公式)

### **题意**

给定一组数$$a$$，要求支持两种操作：

1、将$$[l, r]$$中的数加上$$x$$

2、求$$\sum_{i = l}^{r} f(a[i])$$，其中$$f(x)$$是第$$x$$个斐波那契数。

$$n,m  <= 100000$$

### **题解**

斐波那契数的第$$i$$项就是$$
 \left[
 \begin{matrix}
   0 & 1 \\
   1 & 1
  \end{matrix}
  \right]
$$的$$i$$次方左下角的元素。

对每个数存下对应的矩阵，求和相当于矩阵求和，区间加相当于区间乘一个矩阵，复杂度为$$O(nlogn)$$

[1]: http://codeforces.com/contest/718/problem/C