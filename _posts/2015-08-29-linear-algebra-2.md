---
layout: post_latex
title: <复习向>线性代数之矩阵与行列式(2)
tags: ['matrix','linear algebra']
published: true
---

###行列式的意义

貌似一般的线性代数教科书并没有告诉读者行列式的实际意义，只是教会了读者行列式的定义和计算方法。（起码我所阅读的线性代数课本没有提及）

那么在这里我简单地介绍一下。

##一阶行列式

要说行列式的意义，得先从行列式的"|"符号谈起。下面是一阶方阵的行列式：

\\[ |x| = x \\]

是不是想到什么？一阶方阵，其实就是一个数，且它的行列式等于这个数。且，一阶方列式的写法，恰好就是高中数学里的绝对值写法！

想一下绝对值的**几何意义**：指明了一个实数（这里不提虚数）距离数轴原点的大小。



##二阶行列式

现在看一下二阶行列式：

{% assign detmat = "x\_\{0\},x\_\{1\},y\_\{0\},y\_\{1\}" | split: ',' %}

{% include render_det.html mat = detmat row = 2 col = 2 %}

再变成用向量来表示：

\\( |\ \\alpha\ \\beta\ | \\)

于是，二阶行列式等于2个向量的"绝对值"。那么，对于2个向量，这个绝对值是什么？

首先，搬出向量的夹角公式：

\\[ cos\\theta = \\dfrac \{\\alpha \\cdot\ \\beta \} \{|\\alpha |\\times|\\beta |\} \\]

从上面的式子可以推出：

\\[ sin\\theta = \\sqrt\{1 - \\dfrac \{(\\alpha \\cdot\ \\beta )\^\{2\}\} \{|\\alpha |\^\{2\}\\times |\\beta |\^\{2\}\} \} \\]


\\[ |\\alpha |\\times |\\beta |\\times sin\\theta = \\sqrt\{ |\\alpha |\^\{2\} \\times |\\beta |\^\{2\} - (\\alpha \\cdot\ \\beta )\^\{2\} \} \\]

\\[ |\\alpha |\\times |\\beta |\\times sin\\theta = \\sqrt\{ (x\_\{0\}\^\{2\} + y\_\{0\}\^\{2\})\\times (x\_\{1\}\^\{2\} + y\_\{1\}\^\{2\}) - (x\_\{0\}x\_\{1\}+y\_\{0\}y\_\{1\})\^\{2\} \} \\]

\\[ |\\alpha |\\times |\\beta |\\times sin\\theta = \\sqrt\{ x\_\{0\}\^\{2\}x\_\{1\}\^\{2\} + x\_\{0\}\^\{2\}y\_\{1\}\^\{2\} + y\_\{0\}\^\{2\}x\_\{1\}\^\{2\} + y\_\{0\}\^\{2\}y\_\{1\}\^\{2\} - x\_\{0\}\^\{2\}x\_\{1\}\^\{2\} - x\_\{0\}x\_\{1\}y\_\{0\}y\_\{1\} - y\_\{0\}\^\{2\}y\_\{1\}\^\{2\} \} \\]

\\[ |\\alpha |\\times |\\beta |\\times sin\\theta = \\sqrt\{ x\_\{0\}\^\{2\}y\_\{1\}\^\{2\} + x\_\{1\}\^\{2\}y\_\{0\}\^\{2\} - 2x\_\{0\}x\_\{1\}y\_\{0\}y\_\{1\} \} \\]

\\[ |\\alpha |\\times |\\beta |\\times sin\\theta = \\sqrt\{ (x\_\{0\}y\_\{1\} - x\_\{1\}y\_\{0\})\^\{2\} \} \\]

\\[ |\\alpha |\\times |\\beta |\\times sin\\theta = x\_\{0\}y\_\{1\} - x\_\{1\}y\_\{0\} \\]

\\[ |\\alpha |\\times |\\beta |\\times sin\\theta =  {% include render_det_raw.html mat = detmat row = 2 col = 2 %} = |\ \\alpha\ \\beta\ | \\]

注意到了吗，这个式子的左边，赫然是平行四边形的面积公式！

所以，二阶行列式的几何意义就是**2个向量组成的平行四边形的面积**。

![parallelogram.png](../images/2015.8/parallelogram.svg)


##三阶行列式
