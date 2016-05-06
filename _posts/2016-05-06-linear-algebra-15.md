---
layout: post_latex
title: 最小二乘估计(Least Squares Estimator)的公式的推导
tags: ['matrix','linear algebra','Least Squares']
published: true
---



最近在学习ML(machine learning)，注意到了一个有趣的东西：[Least Squares Estimator](https://en.wikipedia.org/wiki/Linear_least_squares_\(mathematics\))。

先从简单说起吧。看下面的式子：

\\[ y = ax + e \\]

这是一个非常简单的直线方程。如果赋予y、a、x、b具体的意义，这个式子就有意思了：

1. 假设x是一个统计变量（预先就知道的），譬如，x代表人的年龄。

2. 假设y是关于x的一个label量（预先就知道的），譬如，y代表的是年龄为x时的人的智商。


<!--more-->


3. 假设y和x存在线性关系，那么可以有 y = ax。这个式子表明年龄为x时，智商为ax。

4. 当x、y的取值只有一对时，a = y/x，但当x、y不只一对时，y = ax可能会无解（因为求解的是方程组 \\( y\_\{i\} = ax\_\{i\} \\) 了）

5. 为了使方程组 \\( y\_\{i\} = ax\_\{i\} \\) 可以求解，需要把方程组扩展成 \\( y\_\{i\} = ax\_\{i\} + e\_\{i\} \\) 。

6. \\( y\_\{i\} = ax\_\{i\} + e\_\{i\} \\)使得我们有机会求出a，但同时也产生了很多个\\( e\_\{i\} \\)。每对<y,x>都有它自己的error系数的话，这个a的意义就减弱了。

7. 为了使得a变得更有意义，我们希望每个error系数尽可能地小（无限逼近0最好了），同时又能求出唯一的a。

8. 又因为现实生活中，智商肯定不只跟年龄x有关系，还和其他参数有关系，那么可以再把公式扩展成:

\\[ y\_\{i\} = a\_\{1\}x\_\{i1\} + a\_\{2\}x\_\{i2\} + \\cdots + a\_\{k\}x\_\{ik\} + e\_\{i\} , 1\\le i\\le n, k\\ge 1 \\]

现在，把上式写成矩阵形式：

\\[ \\vec y = X\\vec a + \\vec e \\]


{% assign A =  "a\_\{1\},a\_\{2\},\\vdots ,a\_\{k\}" | split: ',' %}
{% assign Y =  "y\_\{1\},y\_\{2\},\\vdots ,y\_\{n\}" | split: ',' %}
{% assign E =  "e\_\{1\},e\_\{2\},\\vdots ,e\_\{n\}" | split: ',' %}

{% assign X =  "x\_\{11\},x\_\{12\},\\cdots ,x\_\{1k\},   x\_\{21\},x\_\{22\},\\cdots ,x\_\{2k\},   \\vdots ,\\vdots ,\\ddots ,\\vdots ,  x\_\{n1\},x\_\{n2\},\\cdots ,x\_\{nk\}" | split: ',' %}


\\[ {% include render_matrix_raw.html mat = Y  row = 4 col = 1 %} = {% include render_matrix_raw.html mat = X row = 4 col = 4 %}{% include render_matrix_raw.html mat = A  row = 4 col = 1 %} + {% include render_matrix_raw.html mat = E  row = 4 col = 1 %} \\]

再回到上面的第7点：为了使得\\(\\vec a\\)变得更有意义，我们希望\\(\\vec e\\)的每个分量尽可能地小。明确这一点非常重要。

那么，这个目标完成情况应该如何衡量？其实很简单，既然\\(\\vec e\\)是一个向量（n维空间），那么\\(\\vec e\\)的长度就是我们需要的指标：


\\[ |\\vec e| = \\sqrt \{ \\sum \^\{n\}\_\{i=1\}e\_\{i\}\^\{2} \} \\]

开根号是不必要的，我们可以换成下面这个指标：

\\[ |\\vec e|\^\{2} = \\sum \^\{n\}\_\{i=1\}e\_\{i\}\^\{2} = \\vec e\\vec e = \\vec e\^\{T\}\\vec e \\]

**小结一下：当\\( \\vec e\^\{T\}\\vec e \\)取得最小值时，\\(\\vec a\\)能取得最优解。**

继续推导。

由上文可知：

\\( \\vec e = \\vec y - X\\vec a \\)

\\( \\vec e\^\{T\} = (\\vec y - X\\vec a)\^\{T\} \\)

\\( \\vec e\^\{T\}\\vec e = (\\vec y - X\\vec a)\^\{T\}(\\vec y - X\\vec a)  \\)

\\( = (\\vec y\^\{T\} - \\vec a\^\{T\}X\^\{T\})(\\vec y - X\\vec a)  \\)

\\( = \\vec y\^\{T\}\\vec y - \\vec a\^\{T\}X\^\{T\}\\vec y - \\vec y\^\{T\}X\\vec a + \\vec a\^\{T\}X\^\{T\}X\\vec a \\)

注意，中间的2个子项是可以合并的。首先，仔细观察\\( \\vec a\^\{T\}X\^\{T\}\\vec y \\)这个子项，发现它是一个**值**，那么就有：

\\( \\vec a\^\{T\}X\^\{T\}\\vec y  = (\\vec a\^\{T\}X\^\{T\}\\vec y)\^\{T\} \\)

（一个数值可认为是一个1维的方阵，1维方阵的转置矩阵是它本身）

而又有：

\\( (\\vec a\^\{T\}X\^\{T\}\\vec y)\^\{T\} = \\vec y\^\{T\}(\\vec a\^\{T\}X\^\{T\})\^\{T\} \\)

\\( = \\vec y\^\{T\}(X\\vec a) = \\vec y\^\{T\}X\\vec a  \\)

得：

\\( \\vec a\^\{T\}X\^\{T\}\\vec y  = \\vec y\^\{T\}X\\vec a  \\)

所以上面的方程可变为：

\\[ \\vec e\^\{T\}\\vec e = \\vec y\^\{T\}\\vec y - 2\\vec y\^\{T\}X\\vec a + \\vec a\^\{T\}X\^\{T\}X\\vec a \\]

如何让\\( \\vec e\^\{T\}\\vec e \\)取得最小值？此时需要使用新的招数：**矩阵微分**。


## 矩阵微分

矩阵微分公式：

设：

\\[ \\vec y = A\\vec x \\]

y是一个\\(m \\times 1\\)的矩阵，A是一个\\(m \\times n\\)的矩阵，x是一个\\(n \\times 1\\)的矩阵。

则有：

\\[ \\frac \{\\partial \\vec y\}\{\\partial \\vec x\} = A 【公式1】 \\]

这是如何得到的呢？实际上超级简单，上面这个式子指的是，\\(\\vec y \\)的每一个分量对\\(\\vec x \\)的每一个分量的微分，结果显然就是一个\\(m \\times n\\)矩阵。

扩展公式：

设：

\\[ \\alpha = \\vec y\^\{T\}A\\vec x \\]

则有：

\\[ \\frac \{\\partial \\alpha \}\{\\partial \\vec x\} = \\vec y\^\{T\}A  【公式2】 \\]


\\[ \\frac \{\\partial \\alpha \}\{\\partial \\vec y\} = \\vec x\^\{T\}A\^\{T\}  【公式3】 \\]


设：

\\[ \\alpha = \\vec x\^\{T\}A\\vec x \\]

且A是对称矩阵，

则有：

\\[ \\frac \{\\partial \\alpha \}\{\\partial \\vec x\} = 2\\vec x\^\{T\}A  【公式4】 \\]




## 应用矩阵微分公式


再来看下刚才的\\( \\vec e\^\{T\}\\vec e  \\)方程：

\\[ \\vec e\^\{T\}\\vec e  = \\vec y\^\{T\}\\vec y - 2\\vec y\^\{T\}X\\vec a + \\vec a\^\{T\}X\^\{T\}X\\vec a \\]

对等号右边的式子求关于\\(\\vec a\\)的微分，得到：

\\( \\frac \{\\partial \\vec y\^\{T\}\\vec y\}\{\\partial \\vec a\} - 2\\frac \{\\partial \\vec y\^\{T\}X\\vec a\}\{\\partial \\vec a\} + \\frac \{\\partial \\vec a\^\{T\}X\^\{T\}X\\vec a\}\{\\partial \\vec a\} \\)

**当这个式子(导数)等于0时, 就得到了\\( \\vec e\^\{T\}\\vec e \\)的最小值。**

显然，第一个子项为0，所以可把它去掉，得到：

\\( - 2\\frac \{\\partial \\vec y\^\{T\}X\\vec a\}\{\\partial \\vec a\} + \\frac \{\\partial \\vec a\^\{T\}X\^\{T\}X\\vec a\}\{\\partial \\vec a\}  = 0\\)


\\(  2\\frac \{\\vec y\^\{T\}X\\vec a\}\{\\partial \\vec a\} = \\frac \{\\vec a\^\{T\}X\^\{T\}X\\vec a\}\{\\partial \\vec a\} \\)

观察左边的式子，和上文的【公式2】是一样的，所以有：

\\( 2\\frac \{\\vec y\^\{T\}X\\vec a\}\{\\partial \\vec a\} = 2\\vec y\^\{T\}X \\)

观察右边的式子，符合上文的【公式4】，所以有：

\\(  \\frac \{\\vec a\^\{T\}X\^\{T\}X\\vec a\}\{\\partial \\vec a\} = 2\\vec a\^\{T\}X\^\{T\}X \\)

综上，得：

\\(  2\\vec y\^\{T\}X = 2\\vec a\^\{T\}X\^\{T\}X \\)

\\(  \\vec y\^\{T\}X = \\vec a\^\{T\}X\^\{T\}X \\)

\\(  (\\vec y\^\{T\}X)\^\{T\} = (\\vec a\^\{T\}X\^\{T\}X)\^\{T\} \\)

\\(  X\^\{T\}\\vec y = X\^\{T\}X\\vec a \\)

\\(  \\vec a = (X\^\{T\}X)\^\{-1\}X\^\{T\}\\vec y  \\)

这个东西就是所谓的**最小二乘估计(Least Squares Estimator)**了。


## 参考资料

[https://economictheoryblog.com/2015/02/19/ols_estimator/](https://economictheoryblog.com/2015/02/19/ols_estimator/)

[http://www.atmos.washington.edu/~dennis/MatrixCalculus.pdf](http://www.atmos.washington.edu/~dennis/MatrixCalculus.pdf)