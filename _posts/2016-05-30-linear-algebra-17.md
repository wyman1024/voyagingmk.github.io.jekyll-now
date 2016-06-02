---
layout: post_latex
title: 线性代数之主成分分析(PCA)算法
tags: ['matrix','math']
published: true
---

PCA(Principal Component Analysis)的主要应用场景是：在大数据集中找出关键的信息并剔除冗余的信息。根据这个特性，PCA也可以用来做信息压缩(有损)、特征提取。不过在本文中，只会对PCA的数学原理进行阐述。

另外，PCA可以说是Machine Learning领域的自编码机(AutoEncoder,AE)的基础。主要区别在于，PCA是线性算法，而AE则不一定。所以在学习AutoEncoder之前，有必要先将PCA搞清楚。

<!--more-->

# Part I

设向量\\( \\vec x \\)的每个分量分别记录了某一个特征的信息，且共有n个分量(特征)；那么m个不同的\\( \\vec x \\)就组成了一个\\(m\\times n \\)的矩阵\\( X \\)：

{% assign X =  "\\vec x\_\{1\},  \\vec x\_\{2\},  \\vdots , \\vec x\_\{m\}" | split: ',' %}

\\[ X = {% include render_matrix_raw.html mat = X  row = 4 col = 1 %}  \\]

然后问题来了：\\( \\vec x \\)的每个分量之间是否是**相互独立(independant)**的？如果是，那么说明这n个特征是良好的，可以直接拿去应用到任务中(譬如基于这些特征做一个分类器)；如果不是，那么就说明有特征是多余的，譬如\\( x\^\{(k)\} \\)、\\( x\^\{(k+1)\} \\)分别用米和英尺记录了同一个特征，虽然数值不一样，然而并没有什么卵用。

量化特征与特征之间的关系的最好办法是用**方差**([Variance](https://en.wikipedia.org/wiki/Variance))和**协方差**([Covariance](https://en.wikipedia.org/wiki/Covariance))，这2者又共同涉及到了更基础的概念**数学期望**([Expected Value](https://en.wikipedia.org/wiki/Expected_value))和**均值**([Mean](https://en.wikipedia.org/wiki/Mean))。先简单过一遍这4个东西的公式。

### 数学期望和均值

\\[ E[\\vec x] = \\sum \_\{i=1\}\^\{n\}x\_\{i}p\_\{i\} \\]

当每个\\( x\_\{i\} \\)的出现概率相等时(均匀分布)，有\\( p\_\{i\} = \\frac \{1\}\{n\} \\)，所以上式可简化成:

\\[ E[\\vec x] = \\frac \{1\}\{n\}\\sum \_\{i=1\}\^\{n \}x\_\{i} \\]

上式其实也就是均值\\( \\overline \{x\} \\)的定义，所以当\\(x\_\{i\}\\)均匀分布时，有：

\\[  E[\\vec x] =  \\overline \{x\} \\]

### 方差


\\ Var(\\vec x) = E[()]