---
layout: post_latex
title: 线性代数之奇异值分解 SVD Decomposition
tags: ['matrix','linear algebra']
published: true
---

在线性代数中，SVD是对实数矩阵(甚至复数矩阵)的一种因式分解。在信号、统计、图像图形学中都有应用。

SVD非常强大且实用，因为数学界前辈已经证明任意的一个矩阵都可以做SVD分解。这一点特别重要，因为相比SVD分解，和SVD相近的特征值分解只能应用于方阵。

<!--more-->

## SVD的定义

先给出公式的全貌：

设有一个m X n的矩阵M，它的SVD分解是：

\\[ M = UΣV\^\{*\} \\]

其中：

- U是一个m X m的单式矩阵([Unitary Matrix](https://en.wikipedia.org/wiki/Unitary_matrix))
- Σ是m X n的矩形对角矩阵([Rectangular Diagonal Matrix](https://en.wikipedia.org/wiki/Diagonal_matrix))，并且在对角线上的元素都是非负实数\\(\\sigma \_\{i\}\\)，称为M的奇异值
- V*是一个n X n的单式矩阵，也是V的共轭转置矩阵([Conjugate Transpose Matrix](https://en.wikipedia.org/wiki/Conjugate_transpose))

一些补充:

1. U矩阵的m个列向量、V的n个列向量分别被称为M的左奇异向量，和M的右奇异向量
2. 约定Σ矩阵的对角线上的奇异值\\(\\sigma \_\{i\}\\)用降序排列
3. 由第2点可以看出，Σ矩阵完全由M决定，和U、V无关


## SVD和特征值分解的联系

- M的左奇异向量 是 \\(MM\^\{\*\}\\)的特征向量
- M的右奇异向量 是 \\(M\^\{\*\}M\\)的特征向量
- M的非零奇异值（即Σ的对角线上的元素）分别是\\(M\^\{\*\}M\\)以及\\(MM\^\{\*\}\\)的所有非零特征值的开平方

由单式矩阵的定义，知：

\\[ U\^\{*\}U = I = U\^\{-1\}U \\]

\\[ V\^\{*\}V = I = V\^\{-1\}V \\]

\\[ U\^\{*\} = U\^\{-1\} \\]

\\[ V\^\{*\} = V\^\{-1\} \\]

由矩形对角矩阵的定义，知：

\\[ Σ\_\{m,n\}Σ\_\{m,n\}\^\{*\} = Σ\_\{m,n\}Σ'\_\{n,m\} = D\_\{m,m\} \\]

\\[ Σ\_\{m,n\}\^\{*\}Σ\_\{m,n\} = Σ'\_\{n,m\}Σ\_\{m,n\} = D\_\{n,n\} \\]

(D = Diagonal Square Matrix)

即，Σ和Σ的转置相乘，等于一个新的方阵D，D的阶数等于左边的Σ矩阵的行数；D还是一个对角方阵，且对角线上的元素分别是Σ的对角线上的元素的平方。

再根据SVD公式：

\\[ M = UΣV\^\{*\} \\]

有：

\\[ M\^\{\*\}M = V\\Sigma \^\{\*\}U\^\{\*\}U\\Sigma V\^\{\*\} = V\\Sigma \^\{\*\}\(U\^\{\*\}U\)\\Sigma V\^\{\*\} = V\\Sigma \^\{\*\}\\Sigma V\^\{\*\} = V\(\\Sigma  \^\{\*\}\\Sigma \)V\^\{-1\}  \\]

\\[ MM\^\{\*\} = U\\Sigma V\^\{\*\}V\\Sigma \^\{\*\}U\^\{\*\} = U\\Sigma \(V\^\{\*\}V\)\\Sigma \^\{\*\}U\^\{\*\} = U\\Sigma \\Sigma \^\{\*\}U\^\{\*\} = U\(\\Sigma \\Sigma \^\{\*\}\)U\^\{-1\} \\]


右边的东西符合特征分解的定义，所以上述两式子都是特征分解。


## SVD的求法



## 参考资料


[https://en.wikipedia.org/wiki/Singular_value_decomposition](https://en.wikipedia.org/wiki/Singular_value_decomposition)

