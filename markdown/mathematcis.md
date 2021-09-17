# 简单的数学

[TOC]

## 三角函数

### 引言：复指数函数与三角函数

欧拉公式：

$$e^{i\alpha} = \cos\alpha + i \sin\alpha$$

### 和差角公式

两边相乘，有：

$$e^{i(\alpha + \beta)} = e^{i\alpha} \cdot e^{i\beta}$$

$$\Rightarrow$$

$$
\begin{aligned}
    \cos(\alpha + \beta) + i \sin(\alpha + \beta) &= \left( \cos\alpha + i \sin\alpha \right) \left( \cos\beta + i \sin\beta \right) \\
    &= \left( \cos\alpha\cos\beta - \sin\alpha\sin\beta \right) + i \left( \sin\alpha\cos\beta + \cos\alpha\sin\beta \right)
\end{aligned}
$$

由于复数相等的条件是实部、虚部分别相等，因此得到：

$$
\left\{
    \begin{aligned}
        \cos(\alpha + \beta) &= \cos\alpha\cos\beta - \sin\alpha\sin\beta \\
        \sin(\alpha + \beta) &= \sin\alpha\cos\beta + \cos\alpha\sin\beta
    \end{aligned}
\right.
$$


### 诱导公式

将$\displaystyle{\beta = \pm \pi, \pm \frac{\pi}{2}}$分别带入和差角公式，可以得到以下两组常用公式：

$$
\left\{
    \begin{aligned}
        \cos(\alpha + \frac{\pi}{2}) &=  - \sin\alpha \\
        \cos(\alpha - \frac{\pi}{2}) &=  \sin\alpha \\
        \sin(\alpha + \frac{\pi}{2}) &=  \cos\alpha \\
        \sin(\alpha - \frac{\pi}{2}) &=  - \cos\alpha \\
    \end{aligned}
\right.
$$

$$
\left\{
    \begin{aligned}
        \cos(\alpha \pm \pi) &=  - \cos\alpha \\
        \sin(\alpha \pm \pi) &=  - \sin\alpha \\
    \end{aligned}
\right.
$$

### 倍角公式

令$\alpha = \beta$，带入和差角公式，得到：

$$
\left\{
    \begin{aligned}
        \cos(2 \alpha) &=  \cos^2\alpha - \sin^2\alpha \\
        \sin(2 \alpha) &=  2 \cos\sin\alpha \\
    \end{aligned}
\right.
$$

### 和差化积公式

令$\displaystyle{a = \frac{\alpha + \beta}{2}}$，$\displaystyle{b = \frac{\alpha - \beta}{2}}$




## 极限

### $\epsilon-\delta$语言、实无穷与潜无穷
实无穷：一个完成了的、整体的无穷
潜无穷：未完成的、处于不断构造进程之中的无穷

> 取极限过程中的无穷，只是趋于无穷的一种趋势，并不承认无穷本身作为一个实体来存在；极限可以用$\epsilon-\delta$语言来叙述，而这种叙述完全是有限的，不需要牵扯到无穷，无非是用精确的数学语言讲出了“当n任意大时，an与A的差可以任意小”这么个事情而已。

## 从数列到函数

无穷多个离散点
无穷多个连续点
有限个离散点

## 微积分
离散与连续

概率论

矩阵论




最小二乘法求解超定方程