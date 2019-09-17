# 向量分解与函数分解
我们已经熟悉，对于三维欧几里得空间中的向量$\vec{p}$，可以在三个正交基底进行分解$\vec{p}=a\vec{x}+b\vec{y}+c\vec{z}$

$$\displaystyle{x=\sum_{b\in B}\frac{\langle x,b\rangle}{\|b\|^2}b}$$


# （连续）傅里叶变换（CTFT）
函数的变换域是将一个域的函数，借由一组特定的基底函数，变换到另一个域，以便于分析和处理的方法。如在傅里叶变换中，完成的就是一个时域函数（变量为时间$t$）到频域（变量为角频率$\omega$）的变换。变换域的核心思想是使用一组特定的基底函数的组合去拟合原函数。在傅里叶变换中，基底函数为所有频率的复指数函数$e^{j\omega t}$，$\omega \in[-\infty ,\infty]（通常理解为正弦函数即可，链接：欧拉公式）的集合$。如同向量的分解$\vec{z}=a_0\vec{x_0}+a_1\vec{x_1}+\cdots+a_n\vec{x_n}$，基底为$\{\vec{x_0},\vec{x_1},\cdots,\vec{x_n}\}$；傅里叶变换中基底为无穷维的复指数函数，而傅里叶变换所做的工作就是将原函数分解到这个空间的各个基底上去，求出各维度的幅值。

可以记傅里叶变换为：

$$\mathcal{F}:f(t)\rightarrow F(j\omega)$$

计算公式为：

$$\displaystyle{F(j\omega)=\mathcal{F}[f(t)]=\int_{-\infty}^{\infty} f(t)e^{-j\omega t}\,dt}$$

在处理完成之后，采用傅里叶逆变换（ICTFT）将信号变换回来：

$$\mathcal{F}^{-1}:F(j\omega)\rightarrow f(t)$$

计算公式为：

$$\displaystyle{f(t)=\mathcal{F}^{-1}[F(j\omega)]=\frac{1}{2\pi}\int_{-\infty}^{\infty} F(j\omega)e^{j\omega t}\,d\omega}$$

该过程具有直白的物理意义，可以理解为将各频率正弦波叠加得到原信号。

对于一种变换域方法，存在逆变换的条件的条件是基底函数**正交**，否则变换域之后各维度分量存在混叠，无法进行逆变换。

# 离散时间傅里叶变换（DTFT, Discrete Time Fourier Transformation）



# 离散傅里叶变换（DFT, Discrete Fourier Transformation）

## 快速傅里叶变换（FFT, Fast Fourier Transformation）

# 一些数学
## 变换系数的小问题
进行函数分解基底需要满足一定的条件，即**规范正交**。若$\{f_0(x),f_1(x),f_2(x),\cdots\}$为一组基底，各基底是取值在$[a,b]$上的函数，要使其完备正交，需要满足：

$$ 
\left\{
\begin{aligned}
\langle f_i,f_j\rangle=0 &, & i\not= j \\
\langle f_i,f_i\rangle=1
\end{aligned}
\right.
$$

其中$\langle f_i,f_j\rangle$为希尔伯特空间中的向量内积，计算公式为：

$$\displaystyle{\langle f_i(x),f_j(x)\rangle =\int_{a}^{b}f_i(x)\overline{f_j(x)}}\, dx$$

$\overline{f_j(x)}$是$f_j(x)$的复共轭。

对于傅里叶变换的正交基底复指数函数$e^{j\omega t}$，为了满足完备正交的条件，设基底系数为$k$，必须满足：

$$\displaystyle{\int_{-\pi}^{\pi}k^2e^{j(\omega_0-\omega_0 )t}\,dt=1}$$

$$\displaystyle{\int_{-\pi}^{\pi}k^2\,dt=1}$$

$$\displaystyle{k=\frac{1}{\sqrt{2\pi}}}$$

即是说正交基底为$\left \{ \cdots, \dfrac{e^{-2\omega t}}{\sqrt{2\pi}}, \dfrac{-e^{\omega t}}{\sqrt{2\pi}}, \dfrac{1}{\sqrt{2\pi}}, \dfrac{e^{\omega t}}{\sqrt{2\pi}}, \dfrac{e^{2\omega t}}{\sqrt{2\pi}}, \cdots \right \}$（实际上$\omega$取值是连续的）。如同向量的正交分解，各基底上的分量通过$\langle f(t),e^{j\omega t}\rangle$求出。

如果使用这组正交基底的话，傅里叶变换对就可以写为：

$$\displaystyle{F(j\omega)=\int_{-\infty}^{\infty} f(t)\frac{e^{-j\omega t}}{\sqrt{2\pi}}\,dt}$$

$$\displaystyle{f(t)=\int_{-\infty}^{\infty} F(j\omega)\frac{e^{j\omega t}}{\sqrt{2\pi}}\,d\omega}$$

这样就消去了原公式中逆变换的系数$\dfrac{1}{2\pi}$，更加优雅和谐。如要得到原公式,将上式略作变形：

$$
\displaystyle{
\begin{aligned}
f(t) &= \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f(t)\frac{e^{-j\omega t}}{\sqrt{2\pi}}\,dt\frac{e^{j\omega t}}{\sqrt{2\pi}}\,d\omega \\
&=\frac{1}{\sqrt{2\pi}}\frac{1}{\sqrt{2\pi}}\int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f(t){e^{-j\omega t}}\,dte^{j\omega t}\,d\omega \\
&=\frac{1}{2\pi}\int_{-\infty}^{\infty} F(j\omega)e^{j\omega t}\,d\omega
\end{aligned}}$$


即可得到原式的傅里叶反变换，其中：

$$\displaystyle{F(j\omega)=\int_{-\infty}^{\infty} f(t){e^{-j\omega t}}\,dt}$$

为对应的傅里叶正变换。系数的提取仅相当于对得到的频域图形等比例缩放，不会影响单独时域或者频域的分析。习惯上来说，提取系数（即逆变换带系数$\dfrac{1}{2\pi}$）的傅里叶变换公式更为常见。

## 帕塞瓦尔定理（Parseval's Theorem）
基底具备完备正交性有等价命题，即**帕塞瓦尔定理**成立。如同线性空间中一个向量在不同基底表示下长度（范数）依然相同一样，傅里叶变换前后函数的能量（范数）相同，即傅里叶算符是幺正算符

## 频域函数自变量的问题


