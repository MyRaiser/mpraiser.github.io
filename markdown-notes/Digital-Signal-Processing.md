# 数字信号处理

[TOC]

## Z变换
### Z正变换
Z变换对：
$$X(z) = \mathcal{Z}[x(n)] = \sum_{n=-\infty}^{\infty}x[n]z^{-n}$$

### Z反变换
#### 幂级数形式
由Z正变换不难看出，一个时间序列$x(n)$的z变换为一$z^{-1}$的n次多项式，其中每项的系数恰为时间序列对应点的值。通常$X(z)$（符合因果性）都可以展开成这样的形式：

$$X(z) = x[0](z^{-1})^0 + x[1](z^{-1})^1 + x[2](z^{-1})^2 + \cdots$$

这样通过系数可以直接得到原时间序列。

展开的方法可以参考： [长除法](https://baike.baidu.com/item/%E5%A4%9A%E9%A1%B9%E5%BC%8F%E9%95%BF%E9%99%A4%E6%B3%95/18881169?fr=aladdin)

需要注意的是，这种方法得到的是原序列的数值形式，而非解析形式。对于有限长的原序列才比较合适这种方法。

#### 有理分式部分分式分解
Z变换除了幂级数形式非常好反变换，写成这样的形式也可以简单的找到对应的原序列：

$$F(z) = \frac{1}{1-az^{-1}} = \frac{z}{z-a}$$

对应原函数如表：

时域函数$x[n]$ | 频域函数$X[z]$
:-: | :-: | :-:
$$a^n\epsilon[n]$$ | $$\frac{z}{z-a}$$
$$na^{n-1}\epsilon[n]$$ | $$\frac{z}{(z-a)^2}$$ 

通常（有理函数）可以将$F(z)$写成这样的形式，进行部分分式分解得到若干个这样的项：
$$\frac{F(z)}{z} = \frac{B_0}{z} + \frac{B_1}{z-v_1} + \frac{B_2}{z-v_2} + \cdots$$

再两边同乘$z$，得到上述形式项之和。这样就可以转变为简单形式之和从而方便的得到原序列了。

这个展开的方法称作部分分式展开，记有一有理函数$G(z)$，可以写成：

$$G(z) = \frac{N(z)}{D(z)} = \frac{N(z)}{(z-z_1)(z-z_2)\cdots(z-z_n)} = \frac{K_1}{z-z_1} + \cdots + \frac{K_n}{z-z_n}$$

> 在Z变换中，通常先做$G(z) = \dfrac{F(z)}{z}$

这里先讨论$D(z)$无重根的情况。$N(z)$和$D(z)$均为$z$的多项式。需要注意的是为了方便通过变换令$D(z)$的$z^n$项系数为$1$，并且$G(z)$必须为真分式（即分子次数比分母低），否则要先通过长除法将假分式转化为多项式与真分式之和。

部分分式展开的过程即求取待定系数$K_1,K_2,\cdots,K_n$，如果表达式较为简单的话，可以直接用**待定系数法**求解。

下面给出一个求取系数的通用公式。如果对上式左右同乘$(z-z_i)$，有：

$$(z-z_i)\frac{N(z)}{D(z)} = K_i + (z-z_i)\sum_{k\not=i}\frac{K_k}{z-z_k}$$

取$z=z_i$的时候，右边第二项就是$0$了，只剩下$K_i$，因此可以得到无重根项的系数求解公式：

$$K_i = \lim_{z\to z_i}[(z-z_i)\frac{N(z)}{D(z)}]$$

需要注意的是取等只能通过逼近取得，是一个$\dfrac{0}{0}$型不定式，通常该极限要通过洛必达法则求取：

$$\begin{aligned}
    K_i &= \lim_{z\to z_i}[(z-z_i)\frac{N(z)}{D(z)}]\\
    &= \lim_{z\to z_i}\frac{\frac{d}{dz}[(z-z_i)N(z)]}{\frac{d}{dz}[D(z)]}\\ 
    &= \lim_{z\to z_i}[\frac{N(z)}{D'(z)}]
\end{aligned}$$

对于$p$阶重根的项，有相似的思路，左右同乘$(z-z_i)^p$：

$$\begin{aligned}
    (z-z_i)^p\frac{N(z)}{D(z)} &= (z-z_i)^p \left[ \frac{K_{i,p}}{(z-z_i)^p} + \frac{K_{i,p-1}}{(z-z_i)^{p-1}} +\cdots+ \frac{K_{i,0}}{(z-z_i)} + \sum_{k\not=i}\frac{K_k}{z-z_k} \right]\\
    &= K_{i,p} + K_{i,p-1}(z-z_i) +\cdots+ K_{i,0}(z-z_i)^{p-1} + (z-z_i)^p\sum_{k\not=i}\frac{K_k}{z-z_k}
\end{aligned}$$

可见要求得$K_{i,p-j}$只需要对上式两边求$j$次导，由此得到$p$阶重根的项的系数公式：

$$K_{i,p-j} = \lim_{z\to z_i}\left\{
    \frac{1}{j!}
    \frac{d^j}{dz^j}\left[
        (z-z_i)^p\dfrac{N(z)}{D(z)}
        \right]
    \right\}$$
#### 留数法
我还不会

### 收敛域
- 不能包含极点，以极点为界。
- 收敛域与序列关系的性质：
    1. $M\leq n \leq N$的有限长序列覆盖整个$z$平面，但有可能要去掉$z=0$或$z=\infty$
    2. $M\leq n < \infty$右边无限长序列在距离原点最远极点的圆外部
    3. $\infty < n \leq N$左边序列在距离原点最近极点的圆内部
    4. 无限长双边序列为环形

### 使用Z变换分析系统性质
#### 稳定性
系统传输函数$H(z)$收敛域包括单位圆，即$H(e^{j\omega})$存在。

如果考虑的是一右边序列（通常都是），进一步有所有极点均在单位圆内。

#### 因果性
系统传输函数$H(z)$收敛域包括$z=\infty$。

这一性质可以如此理解：如果$H(z)$在$z=\infty$处极点，即意味着$H(z)$有因子$z^p$，由上文幂级数性质可知此时原序列$h[n]$在$n<0$时有值。

## 平稳随机信号基础
### 自相关函数与互相关函数
定义：

$$R_{xy}(m) = E[x(n)y(n+m)]$$
#### 性质


### 功率谱

### 平稳随机信号通过线性系统
有一平稳随机信号$x[n]$，通过线性系统$h[n]$，得到$y[n]$。

有：

$$R_{yy}(m) = R_{xx}(m) \ast h[m] \ast h[-m]$$

$$\begin{aligned}
    P_{yy}(z) &= P_{xx}(z)H(z)H^*(z)\\
    &= P_{xx}(z)H(z)H(z^{-1})\\
    &= P_{xx}(z) |H(z)|^2
\end{aligned}$$

对于$x$和$y$互相关，有（xy顺序？PPT定义？书上定义？）：

$$P_{yx}(z) = P_{xx}(z)H(z)$$

$$P_{xy}(z) = P_{xx}(z)H^*(z)$$

## 谱分解
对于任意实平稳随机信号$y_n$的有理功率谱$S_{yy}(z)$可以唯一的表示为：

$$S_{yy}(z) = \sigma_\epsilon^2 B(z)B(z^{-1})$$

其中$B(z)$为一有理函数，满足：

$$B(z) = \frac{N(z)}{D(z)}$$

$N(z)$和$D(z)$均为满足最小相位性质的多项式。$\sigma_\epsilon^2$用以调整常系数使得$N(z)$和$D(z)$最高次系数都为$1$。

### 白噪声通过线性系统
$X(z) = \sigma_\epsilon^2$可以看作是一白噪声的功率谱，$\sigma_\epsilon^2 B(z)B(z^{-1})$可以视作白噪声通过线性系统后的功率谱。

这表明：任何平稳随机信号$y_n$都可以看作是由一个白噪声序列激励稳定因果线性时不变系统$B(z)$产生的输出。

>　最小相位：零点都在单位圆内

## 维纳滤波器与卡尔曼滤波器
### 问题模型
输入信号$x(n)=s(n)+v(n)$通过系统$h(n)$得到输出$y(n)$。

其中$s(n)$是原信号，$v(n)$是噪声，滤波器的目标是在带噪声的信号中滤出原信号，即使得$y(n)=\hat{s}(n)$。并且有：

$$\hat{s}(n) = x(n)\ast h(n) = \sum_i h(i)x(n-i)$$

定义估计误差：

$$e(n) = s(n) - \hat{s}(n)$$

滤波器应当按照最小均方误差（MMSE）准则最小化误差：

$$\xi(n) = E[e^2(n)]$$

设计维纳滤波器的过程即设计滤波器系数$h(m)$，令：

$$\frac{\partial\xi(n)}{\partial h(m)} = E[2e(n)\frac{\partial e(n)}{\partial h(m)}] = -2E[e(n)x(n-m)] = 0$$

其中
$$E[e(n)x(n-m)] = 0$$

称为正交方程。表明任何时候估计误差都与输入数据正交。

带入前若干式得：

$$E\{\left[s(n)-\sum_i h(i)x(n-i)\right]x(n-m)\} = 0$$

变化即：
$$E[x(n-m)s(n)] = \sum_i h(i)E[x(n-m)x(n-i)]$$

$$R_{xs}(m) = \sum_i h(i)R_{xx}(m-i)$$

> 在胡广. 数字信号处理理论算法与实现. 第一版. 中式左侧写成了$R_{sx}(m)$，是一个错误。

这个方程称作维纳-霍夫（Wiener-Hopf）方程。

### 求解维纳-霍夫方程
维纳-霍夫方程没有规定滤波器的形式，滤波器$h(i)$可以取不同取值范围，主要使用的形式有：

1. $0\leq i \leq N-1$，FIR维纳滤波器。
2. $0\leq i < \infty$，因果IIR维纳滤波器。
3. $-\infty < i < \infty$，非因果IIR维纳滤波器。

可用于：

1. 滤波
2. 平滑
3. 预测

#### FIR维纳滤波器

#### 非因果

#### 因果


## 自适应滤波器
### 自适应滤波器基本原理
自适应滤波是维纳滤波器的发展/推广，可以在工作中逐渐估计所需的统计特性。

![](Digital-Signal-Processing/adaptive-filter.png)

$d(n)$称为参考信号，是自适应滤波器的输入之一，有：

$$e(n) = d(n)-y(n)$$

自适应滤波器的目标依然是最小化误差。

记$L+1$长的输入信号：

$$\mathbf{x}(n) = [x(n),x(n-1),\cdots,x(n-L)]^T$$

对于单输入的自适应滤波器，其有$L+1$个参数构成权系数矢量：

$$\mathbf{w}(n) = [w_0(n),w_1(n),\cdots,w_L(n)]^T$$

输出为：

$$y(n) = \mathbf{x}^T(n)\mathbf{w}(n) = \mathbf{w}^T(n)\mathbf{x}(n)$$

均方误差为：

$$\begin{aligned}
    \xi(n) &= E[e^2(n)] = E[d^2(n)+y^2(n)-2d(n)y(n)]\\
    &= E[d^2] + E[(\mathbf{w}^T\mathbf{x})^2] - 2E[d\mathbf{w}^T\mathbf{x}] + E[(\mathbf{w}^T\mathbf{x})^2] \\
    &= E[d^2] + \mathbf{w}^TE[\mathbf{x}\mathbf{x}^T]\mathbf{w} - 2E[d\mathbf{x}^T]\mathbf{w}
\end{aligned}$$

如果记$x(n)$的自相关矩阵为$\mathbf{R}$

$$\mathbf{R} = E[\mathbf{x}(n)\mathbf{x}^T(n)]$$

记$d(n)$与$\mathbf{x}(n)$的互相关矩阵为$\mathbf{P}$

$$\mathbf{P} = E[d(n)\mathbf{x}(n)]$$

$$\xi(n;\mathbf{w}) = E[d^2(n)] + \mathbf{w}^T\mathbf{R}\mathbf{w} - 2\mathbf{P}^T\mathbf{w}$$

这在参数空间$(w_0,w_1,\cdots,w_L)$称作均方误差性能曲面。

求梯度：

$$\nabla\xi = 2\mathbf{R}\mathbf{w}-2\mathbf{P} = 0$$

可以解出最优参数：

$$\mathbf{w}^* = \mathbf{R}^{-1}\mathbf{P}$$

这称作最佳权矢量或维纳解，与FIR维纳滤波器的解一致。

> 但是矩阵求逆计算很复杂，所以有以下多种改进方法。


### GD
$$\mathbf{w}^{(t+1)} = \mathbf{w}^{(t)} - \mu\nabla\xi(n)$$

假设迭代计算频率与采样频率相同，变为：

$$\mathbf{w}(n+1) = \mathbf{w}(n) - \mu\nabla\xi(n)$$

#### 稳定性分析
分析收敛性：

### LMS
最小均方（LMS）算法，不要求脱线计算。

用平方误差代替均方误差，有：

$$\begin{aligned}
    \nabla\xi &\approx \frac{\partial}{\partial\mathbf{w}}(e^2(n))\\
    &= 2e(n)\frac{\partial(d(n)-\mathbf{w}^T\mathbf{x})}{\partial\mathbf{w}} \\
    &= -2e(n)\mathbf{x}(n)
\end{aligned}$$

### RLS
自适应递归最小二乘方（RLS）算法：

用时间平均的最小化准则替代均方误差。对于误差信号增加遗忘因子$\lambda$，误差函数为：

$$\epsilon(n) = \sum_{k=0}^{n}\lambda^{n-k}e^2(k)$$

> 对于非平稳信号，更好的进行跟踪。

求梯度，令：

$$\begin{aligned}
    \nabla\epsilon(n) &= \sum_{k=0}^{n}\lambda^{n-k} \cdot 2 e(k)\frac{\partial(d(k)-\mathbf{x}^T\mathbf{w})}{\partial\mathbf{w}}\\
    &= -2\sum_{k=0}^{n}\lambda^{n-k}[d(k)-\mathbf{x}^T\mathbf{w}]\mathbf{x} = 0
\end{aligned}$$

得到：

$$[\sum_{k=0}^{n}\lambda^{n-k}\mathbf{x}\mathbf{x}^T]\mathbf{w} = \sum_{k=0}^{n}\lambda^{n-k}d(k)\mathbf{x}$$

记：

$$\mathbf{R}(n) = \sum_{k=0}^{n}\lambda^{n-k}\mathbf{x}\mathbf{x}^T$$

$$\mathbf{P}(n) = \sum_{k=0}^{n}\lambda^{n-k}d(k)\mathbf{x}$$

正交方程变为：

$$\mathbf{R}\mathbf{w} = \mathbf{P}$$

最小二乘方准则的维纳解为：

$$\mathbf{w}^* = \mathbf{R}^{-1}\mathbf{P}$$

## 现代谱估计

## 矩阵微积分