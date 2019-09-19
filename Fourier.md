# 有限维空间的向量分解与无穷维空间的函数分解
我们已经熟悉，对于三维欧几里得空间中的向量$\vec{p}$，可以在三个正交基底进行分解$\vec{p}=a\vec{x}+b\vec{y}+c\vec{z}$。其中，$a,b,c$分别是$\vec{p}$在$\vec{x},\vec{y},\vec{z}$上的投影长度。对于欧几里得空间，如果记$B$为正交基底的集合（在三维的例子下即$B=\{\vec{x},\vec{y},\vec{z}\}$），$\vec{b}$为其中一个正交基底，那么向量的正交分解可以写为：

$$\displaystyle{\vec{p}=\sum_{b\in B}\frac{\langle \vec{p},\vec{b}\rangle}{\| \vec{b}\|^2}\vec{b}}$$

现在让我们忽略掉一些数学细节，对于希尔伯特空间中的元素$x$（是一个函数）可以进行类似的操作：

$$\displaystyle{x=\sum_{b\in B}\frac{\langle x,b\rangle}{\|b\|^2}b}$$

这就完成了一个函数在另一组正交基底函数上的分解工作。在傅里叶变换中，基底选择的就是复指数函数$e^{j\omega t}$。
 
# （连续时间）傅里叶变换（CTFT）
函数的变换域是将一个域的函数，借由一组特定的基底函数，变换到另一个域，以便于分析和处理的方法。如在傅里叶变换中，完成的就是一个时域函数（变量为时间$t$）到频域（变量为角频率$\omega$）的变换。变换域的核心思想是使用一组特定的基底函数的组合去拟合原函数，即变换的过程就是求出各正交基底上幅度分量的过程。在傅里叶变换中，基底函数为所有频率的复指数函数$e^{j\omega t}$，$\omega \in[-\infty ,\infty]（通常理解为正弦函数即可，链接：欧拉公式）的集合$。

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
对于离散时间信号，连续傅里叶变换无法直接应用。但是我们可以把离散信号看作连续信号在时间点的抽样，记抽样间隔为$T_s$，连续信号为$f(t)$，离散序列为$x(n)$，有：

$$\displaystyle{x(n)=f(t)\bigg|_{t=nT_s} \simeq f(t)\sum_{n=-\infty}^{\infty}\delta(t-nT_s)}$$

通过$\delta$函数的帮助（太棒了！），我们可以将连续域与离散域无割裂的联系起来。因此我们可以直接把上式带入CTFT，为了避免混淆，我们记模拟角频率为$\Omega$，数字角频率为$\omega$，$\omega=\Omega T_s$，记频域函数为$X(e^{j\omega})$，有^[1]：

$$\displaystyle{\begin{aligned}
    X(e^{j\omega}) & = \int_{-\infty}^{\infty} f(t)\sum_{n=-\infty}^{\infty}\delta(t-nT_s)e^{-j\Omega t}\,dt \\
    & = \sum_{n=-\infty}^{\infty}\int_{-\infty}^{\infty} f(t)e^{-j\Omega t} \delta(t-nT_s)\,dt \\
    & = \sum_{n=-\infty}^{\infty} f(nT_s)  e^{-j\Omega T_{s}n} \text{(}\delta \text{函数的性质)} \\
    \end{aligned}}$$

其中，$f(nT_s)$即为$x(n)$，又有$\omega=\Omega T_s$，则DTFT正变换公式为：

$$\displaystyle{X(e^{j\omega})=\sum_{n=-\infty}^{\infty} x(n)  e^{-j\omega n}}$$

So far so good，DTFT的问题看起来就这样轻易的解决掉了，但是在细枝末节上却隐藏着各种各样的问题。为了避免混淆思维过程，在这里先直接给出正确的IDTFT形式，后文再进行细致讨论（如果有兴趣的话）。IDTFT的正确形式为：

$$\displaystyle{
    x(n)=\frac{1}{2\pi}\int_{-\pi}^{\pi} X(e^{j\omega})e^{j\omega n}\,d\omega 
}$$

对原模拟信号的抽样过程相当于将原信号频谱进行了周期搬移，所以在一个$2\pi$周期内的频谱$X(e^{j\omega})$已经包含有原信号的全部信息了，因此只需对$[-\pi,\pi]$进行积分就可以完全还原原信号。

## 模拟角频率和数字角频率的关系
类型 | 符号| 单位
:-:|:-:|:-:
数字角频率 | $\omega$ | rad
模拟角频率 | $\Omega$ | rad/s

# 离散傅里叶变换（DFT, Discrete Fourier Transformation）

/离散时间傅里叶级数（DTFS）

## 快速傅里叶变换（FFT, Fast Fourier Transformation）
# 再谈（连续时间）傅里叶级数（CTFS）
笔者在写下这份笔记的时候，其实是非常不愿意讨论傅里叶级数的，因为他形式繁琐，又臭又长，让数学苦手的笔者十分头大，而且在实际分析中很少用得上。奈何查阅的绝大多数资料都是从傅里叶级数讲起，逐步推导出傅里叶变换。这对于理解和应用来说既繁琐又不必要。但是，在从CTFT到DTFT，再到DFT的推进过程中，CTFS也逐渐揭示出清晰的属于他的位置。正如DFT实质上也是DTFS，CTFS实质上为**连续时域**到**离散频域**的变换。

变换类型 | 时域性质 | 频域性质
:-:|:-:|:-:
CTFT | 连续 | 连续
CTFS | 连续 | 离散
DTFT | 离散 | 连续
DFT/DTFS | 离散 |离散

# 一些数学
## 变换系数的小问题
进行函数分解基底需要满足一定的条件，首先是**正交**，但最好是**规范正交**（即不仅相互正交，而且各自都是单位向量）。若$\{f_0(x),f_1(x),f_2(x),\cdots\}$为一组基底，各基底是取值在$[a,b]$上的函数，要使其规范正交，需要满足：

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

对于傅里叶变换的正交基底复指数函数$e^{j\omega t}$，为了满足规范正交的条件，设基底系数为$k$，必须满足：

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

为对应的傅里叶正变换。系数的提取仅相当于对得到的频域图形等比例缩放，在反变换时加上去即可，不会影响单独时域或者频域的分析。习惯上来说，提取系数（即逆变换带系数$\dfrac{1}{2\pi}$）的傅里叶变换公式更为常见。

## 帕塞瓦尔定理（Parseval's Theorem）
帕塞瓦尔定理基底具备完备正交性有等价命题，即**帕塞瓦尔定理**成立。如同线性空间中一个向量在不同基底表示下长度（范数）依然相同一样，傅里叶变换前后函数的能量（范数）相同，即傅里叶算符是幺正算符

## 从ICTFT思考IDTFT
使用相同的思路（从CTFT导出DTFT）来考虑IDTFT，直接带入$X(e^{j\omega})$，有：

$$\displaystyle{\begin{aligned}
    & f(t)\sum_{n=-\infty}^{\infty}\delta(t-nT_s) \\
    &=\frac{1}{2\pi}\int_{-\infty}^{\infty} X(e^{j\omega})e^{j\Omega nT_s}\,d\Omega \\
    &=\frac{1}{T_s}\frac{1}{2\pi}\int_{-\infty}^{\infty} X(e^{j\omega})e^{j\omega n}\,d\omega 
    \end{aligned}}$$

可以观察到，该式与正确的IDTFT形式具有区别。区别有二，一是在积分区间，二是在系数。我们在上文提到DTFT频谱中在一个$2\pi$区间中，已经具备了原信号完全的信息，这个结论显然是正确的。那么为什么会产生这种看似矛盾的佯谬呢？

原因在于对模拟信号的抽样这一步：

$$\displaystyle{x(n)=f(t)\bigg|_{t=nT_s} \simeq f(t)\sum_{n=-\infty}^{\infty}\delta(t-nT_s)}$$

可以看到上文中在写抽样这一步时，没有写“$=$”，而是写了“$\simeq$”，原因在于，这两者是有细微差异的。回顾$\delta$函数的定义：

$$\left\{\begin{aligned}
    & \delta (t)=0, x\not=0\\
    & \int_{-\infty}^{\infty} \delta (t)\, dt=1
\end{aligned}\right.$$

可见$\delta$函数在非0的位置均为0，在0的位置是一个无穷大（尽管不能简单的把它当成无穷大，但我们暂且这么表示。后文同理）。这就带来了一个问题：当使用$\delta(t-nT_s)$对$f(t)$进行抽样的时候，在$t=nT_s$处得到的并非是$f(nT_s)$的值，而是以$f(nT_s)$为系数的一个无穷大，只有当积分之后才是$f(nT_s)$本身。因此，上文的推导在带入CTFT时，实际带入的是一串带系数的冲激序列。但是最终得到的公式中的$x(n)$确实是系数的序列，仅包含$\{f(nT_s)\}$本身。这有无矛盾？其实并没有。$x(n)$本身就是对各点抽样值的一个表示，而抽样信号$\delta(t-nT_s)$被隐含了。在做离散信号分析的时候，我们关心的是离散序列本身，至于冲激我们并不关心，只需要对$x(n)$作处理就好了，因此DTFT才脱胎于CTFT。

这给我们探讨IDTFT带来了一点点小麻烦，因为套用ICTFT得到的必然是带系数的冲激序列$f(t)\sum_{n=-\infty}^{\infty}\delta(t-nT_s)$（而我们只想求得$x(n)$），对$[-\infty,\infty]$的积分也确实会导致一个无穷大的冲激。剩下的问题只有系数$\frac{1}{T_s}$。这个系数的由来可以这么解释：

首先我们回到$f(t)$，$f(t)$的频谱为$F(j\Omega)$。不妨记抽样序列$\displaystyle{\hat{f}(t)=f(t)\sum_{n=-\infty}^{\infty}\delta(t-nT_s)}$，有$\hat{f}(t)$的频谱$\hat{F}(j\Omega)=X(e^{j\omega})$。$F(j\Omega)$与$\hat{F}(j\Omega)$之间有什么关系（当然，假设前提是抽样频率足够大。链接：Nyquist采样定理）？在这里我们先引用一傅里叶变换对：

$$\displaystyle{
    \sum_{n=-\infty}^{\infty}\delta(t-nT_s) \rightleftharpoons \frac{2\pi}{T_s}\sum_{n=-\infty}^{\infty}\delta(\Omega-n\Omega_s)
    }$$

由CTFT的时域乘法性质，频域相互卷积，有：

$$\displaystyle{
    \begin{aligned}
    \hat{F}(j\Omega) & = \frac{1}{2\pi}F(j\Omega) * \frac{2\pi}{T_s}\sum_{n=-\infty}^{\infty}\delta(\Omega-n\Omega_s) \\
    & = \frac{1}{T_s}\sum_{n=-\infty}^{\infty}F(j(\Omega-n\Omega_s))
    \end{aligned}}$$

说明$\hat{F}(j\Omega)$是$F(j\Omega)$的周期重复，且幅值下降为$\displaystyle{\frac{1}{T_s}}$，且频谱不混叠的前提下，一个$\Omega \in [-\Omega_s, \Omega_s]$的单个周期内即含有原信号的全频谱。根据数字角频率和模拟角频率的关系$\omega = \Omega T_s$，即在$\omega \in [-2\pi, 2\pi]$的区间即为$f(t)$的全频谱。因此要得到$f(t)$本身，只需对$\hat{F}(j\Omega)$乘以系数$T_s$，在$\omega \in [-2\pi, 2\pi]$进行ICTFT就可以得到$f(t)$。该过程可以写为：

$$\displaystyle{\begin{aligned}
    f(t) &= \frac{1}{2\pi}\int_{-\infty}^{\infty} F(j\Omega)e^{j\Omega t}\,d\Omega\\
    &= \frac{1}{2\pi}\int_{-\infty}^{\infty} \frac{1}{T_s} F(j\Omega)e^{j\Omega t}\,d\omega \\
    &= \frac{1}{2\pi} \int_{-2\pi}^{2\pi} \hat{F}(j\Omega) e^{j\Omega t} \, dw

\end{aligned}}$$

此时你可能会突然迷惑了：绕了这么一大圈，我们的目标究竟是什么？但仔细理一理，可以发现方向并没有歪。我们的最终目标是**通过反变换求得$x(n)$**，而$x(n)$是不含无穷大的抽样冲激的那一个。因此（实质上是）通过ICTFT从$x(n)$的频域函数$X(e^{j\omega})$反求得$f(t)$，再通过$x(n)=f(t)\bigg|_{t=nT_s}$，选取$f(t)$在$t=nT_s$点上的函数值。而$\hat{F}(j\Omega)$实质上就是$X(e^{j\omega})$，至此，我们终于得到正确形式的IDTFT：

$$\displaystyle{\begin{aligned}
    f(t)\bigg|_{t=nT_s} &= \frac{1}{2\pi} \int_{-2\pi}^{2\pi} \hat{F}(j\Omega) e^{j\Omega t} \, dw \bigg|_{t=nT_s} \\
    &= \frac{1}{2\pi} \int_{-2\pi}^{2\pi} X(e^{j\omega}) e^{j\Omega nT_s} \, dw \\
    &= \frac{1}{2\pi} \int_{-2\pi}^{2\pi} X(e^{j\omega}) e^{j\omega n} \, dw \\

\end{aligned}}$$

## 频域函数自变量的问题
我也不知道为什么。

# 拉普拉斯变换（LT, Laplace Transformation ）
LT可以看作是FT的一个推广；或者说，FT是LT的一个特例，他们的区别在于基函数不同。FT的基函数是复指数函数$e^{j\omega t}$，而LT的是带常数的复指数函数$e^{\sigma+j\omega t}$，这使得LT相比于LT具有更强的分析能力，同时有极高的相似性。有：

$$F(s)=\mathcal{L}[f(t)]=$$
 

# Z变换
采用从CTFT推导DTFT同样的方法我们可以简单的从Laplace变换得到Z变换：


## 从Z变换得到DFT

[^1]:这个方法来自《数字信号处理理论算法与实现（胡广书，第3版）》P54-55，对Z变换的推导。
