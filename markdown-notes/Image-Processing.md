- [数字图像处理基础](#数字图像处理基础)
  - [像素间联系](#像素间联系)
    - [像素间距离度量](#像素间距离度量)
    - [邻接](#邻接)
    - [连接](#连接)
    - [连通](#连通)
  - [图像变换](#图像变换)
    - [二维离散傅里叶变换（2-D DFT）](#二维离散傅里叶变换2-d-dft)
      - [矩阵计算形式](#矩阵计算形式)
    - [K-L变换](#k-l变换)
      - [主成分分析（PCA）](#主成分分析pca)
- [图像增强](#图像增强)
  - [基本灰度变换](#基本灰度变换)
  - [锐化](#锐化)
  - [平滑](#平滑)
  - [伪彩色](#伪彩色)
  - [同态滤波](#同态滤波)
  - [颜色迁移](#颜色迁移)
- [边缘检测](#边缘检测)
  - [Canny算子](#canny算子)
    - [高斯滤波器和高斯差分滤波器](#高斯滤波器和高斯差分滤波器)
    - [非极大值抑制](#非极大值抑制)
    - [双阈值检测和连接](#双阈值检测和连接)
- [图像分割](#图像分割)
  - [阈值分割](#阈值分割)
  - [聚类分割](#聚类分割)
  - [主动轮廓](#主动轮廓)
    - [水平集方法](#水平集方法)
      - [演化方程的推导](#演化方程的推导)
        - [$\mathcal{L}_g(\phi)$项的推导](#mathcall_gphi项的推导)
        - [$\mathcal{A}_g(\phi)$项的推导](#mathcala_gphi项的推导)
        - [演化方程](#演化方程)
- [图像回复](#图像回复)

## 数字图像处理基础
### 像素间联系
#### 像素间距离度量
对于两个像素点$p(x,y)$，$q(s,t)$，他们之间的距离需要有一定的度量方法描述，一个统一的描述这一性质的方法是p-范数（p-norm）

$$D(p,q)=\left( |x-s|^p + |y-t|^p \right)^{\frac{1}{p}}$$

常用的典型距离度量函数有：
1. **欧氏距离**
   $$D_E(p,q)=\left( |x-s|^2 + |y-t|^2 \right)^{\frac{1}{2}}$$
   即2-范数。

2. **城区距离**
    $$D_4(p,q)=|x-s| + |y-t|$$
    即1-范数。

3. **棋盘距离**
    $$\begin{aligned}
        D_8(p,q) &= \max(|x-s|,|y-t|) \\
        &= \left( |x-s|^\infty + |y-t|^\infty \right)^{\frac{1}{\infty}}
    \end{aligned}$$
    即$\infty$-范数。该式的证明可以参考：

    -------

    设有一实向量$\boldsymbol{x}=(x_1,x_2,\cdots,x_n)$，其中$x_m$为最大值，即有$x_m=\max_i |x_i|$。要证

    $$\left\|\boldsymbol{x}\right\|_\infty = \max_i |x_i|$$

    有：

    $$\begin{aligned}
        \left\|\boldsymbol{x}\right\|_\infty 
        &= \lim_{p\rightarrow\infty} \left( x_1^p + x_2^p + \cdots + x_n^p\right)^{\frac{1}{p}} \\
        &= \lim_{p\rightarrow\infty} \left[ x_m^p \left( (\frac{x_1}{x_m})^p + (\frac{x_2}{x_m})^p + \cdots + (\frac{x_n}{x_m})^p\right) \right]^{\frac{1}{p}} \\
        &= x_m \lim_{p\rightarrow\infty} \left[ (\frac{x_1}{x_m})^p + (\frac{x_2}{x_m})^p + \cdots + (\frac{x_n}{x_m})^p \right]^{\frac{1}{p}}
    \end{aligned}$$ 

    因为$x_m$为最大值，所以当$x_i \not= x_m$时，$\displaystyle{\frac{x_i}{x_m} < 1}$，有

    $$\lim_{p\rightarrow\infty} \left(\frac{x_i}{x_m}\right)^p = 0$$

    设有$k$项$x_i = x_m$，$k$为一个有限整数，则有：

    $$\begin{aligned}
         \left\|\boldsymbol{x}\right\|_\infty
         &= x_m \lim_{p\rightarrow\infty} \left[0+0+\cdots+k\right]^{\frac{1}{p}} \\
         &= x_m
    \end{aligned}$$

#### 邻接
邻接（adjacency）：
- 主要强调像素间空间关系

1. **4-邻域：**$N_4(p)$
    $$N_4(p)=\{r|D_4(p,r)=1\}$$
2. **8-邻域：**$N_8(p)$
    $$N_8(p)=\{r|D_8(p,r)=1\}$$
3. **对角邻域：**$N_D(p)$

![像素的邻域](Image-Processing/neighbourhood.png)

#### 连接
连接（connectivity）：
- 像素间空间关系（邻接）
- 灰度值满足某个特性相似准则（像素$p$和$r$在同一个灰度值集合$V$取值）

1. **4-连接**
2. **8-连接**
3. **m-连接（混合连接）**
    $p$和$r$都在灰度值集合$V$中取值，且满足下列条件之一：
    - $r$在$N_4(p)$中
    - $r$在$N_D(p)$中，且$N_4(p)\cap N_D(p)$中的像素灰度均不在$V$中

m-连接可以消除8-连接产生的歧义性。

#### 连通
从$p(x,y)$到$q(s,t)$有一系列依次连接的像素组成通路，称作连通。连接是连通的一种特例。

### 图像变换
#### 二维离散傅里叶变换（2-D DFT）
二维傅里叶变换通常指的是二维离散傅里叶变换

##### 矩阵计算形式

#### K-L变换
##### 主成分分析（PCA）

## 图像增强
### 基本灰度变换
### 锐化
### 平滑
### 伪彩色
### 同态滤波
### 颜色迁移

## 边缘检测
![](Image-Processing/edge-model.png)

### Canny算子
Canny算子是用于边缘检测（Edge Detection）的算子，与简单的一阶差分算子（Roberts算子、Sobel算子等）、二阶差分算子（Laplace算子）等相比具有较好的性能，是被广泛应用的边缘检测方法。J. Canny于1986年在*A Computational Approach to Edge Detection*[^canny_paper]中提出Canny算子，并且指出一个好的边缘检测算子应有的三个指标：

[^canny_paper]:J. Canny A Computational Approach to Edge Detection, IEEE Transactions on Pattern Analysis and Machine Intelligence, Vol. 8, No. 6, Nov. 1986. 

> - **低失误概率**
> 既要少将真正的边缘丢失也要少将非边缘判为边缘
> - **高位置精度**
> 检测出的边缘应在真正的边界上
> - **对每个边缘有唯一的响应**
> 得到的边界为单象素宽

Canny算子的实现主要步骤可以分为：

1. 灰度化
2. 高斯平滑滤波
3. 计算梯度大小与方向
4. 非极大值抑制
5. 双阈值检测和连接

#### 高斯滤波器和高斯差分滤波器
高斯滤波器是对图像进行平滑的滤波器。记$r$为平滑半径，$r\in N^+$，则高斯核为一个$(2r+1)\times (2r+1)$的矩阵。高斯核的生成公式为：
$$\displaystyle{
    h=\frac{1}{\sqrt{2\pi \sigma}} e^{-\frac{x^2+y^2}{2\sigma^2}
}}$$

记图像为$f$，对图像先进行高斯卷积滤波，再求梯度。由于卷积算子和差分算子均为**线性算子**，因此可以交换计算顺序，有：
$$\nabla(f \ast h)=(\nabla h)\ast f $$

因此可以得到：
$$\nabla h = (h_x,h_y)=(\frac{\partial h}{\partial x},\frac{\partial h}{\partial y})$$

即意味着高斯滤波器和差分算子可以合成一个算子，称作高斯差分算子（Differiential of Gaussian, DoG）。使用该算子对图像进行处理可以直接得到等效为高级平滑滤波和求差分两步的结果。

对于差分算子在数字图像分析中也有不同的实现形式，如可以使用Sobel算子，则有

$$\displaystyle{
    \frac{\partial}{\partial x} =
    \left[\begin{matrix}
        -1 & 0 & 1 \\
        -2 & 0 & 2 \\
        -1 & 0 & 1
    \end{matrix}\right]
    ,
    \frac{\partial}{\partial y} =
    \left[\begin{matrix}
        1 & 2 & 1 \\
        0 & 0 & 0 \\
        -1 & -2 & -1
    \end{matrix}\right]
}$$

将Sobel算子与高斯核求相关，即可得到高斯差分核。

或者也可以直接对高斯核生成公式求偏微分，可以直接得到高斯差分核的生成公式，有：

$$\displaystyle{\begin{aligned}
    h_x = \frac{\partial h}{\partial x} = \frac{-x}{\sigma^2 \sqrt{2 \pi \sigma}} e^{-\frac{x^2+y^2}{2\sigma^2}} \\
    h_y = \frac{\partial h}{\partial y} = \frac{-y}{\sigma^2 \sqrt{2 \pi \sigma}} e^{-\frac{x^2+y^2}{2\sigma^2}}
\end{aligned}}$$


#### 非极大值抑制
基于最近邻的非极大值抑制方法主要可以分为三步：

1. 将梯度角$\theta[i, j]$的变化范围分为四个扇区
2. 用$3\times 3$邻域作用于幅值图像$M[i, j]$，邻域中心像素$M[i, j]$与沿着梯度线方向的两个像素进行比较
3. 若$M[i, j]$不比沿梯度线方向的两个相邻点幅值大，则像素的(i, j)被抑制, $M[i, j]$被置为$0$。

#### 双阈值检测和连接
双阈值算法采用两个阈值$\tau_1$和$\tau_2$，$\tau_1$为低阈值，$\tau_2$为高阈值。主要步骤为：

1. 对于图像中的像素点，其灰度值低于$\tau_1$的可以判定为一定是边缘，高于$\tau_2$的一定不是边缘。
2. $\tau_2$生成的图像含有假边缘少但有间断点，以$\tau_2$生成的图像为指导，在$\tau_1$的8-邻域内寻找可以连接的点。
3. 重复第二步，直到所有边缘都连接。

## 图像分割
### 阈值分割
### 聚类分割
### 主动轮廓
#### 水平集方法
水平集方法（Level Set）是基于主动轮廓模型的图像分割方法，相比于Snake方法他可以更好的解决重新参数化困难的问题，也能够处理拓扑变化。
##### 演化方程的推导
在水平集方法的应用中，往往多步迭代会使得水平集函数降质（Degraded），对于这种问题常常采用重新初始化的方法。在2005年，不需重新初始化的水平集方法[^1]被提出，这部分将对其演化方程做出推导。

[^1]:Li C, Xu C, Gui C, et al. Level set evolution without reinitialization: a new variational formulation[C]//Computer
Vision and Pattern Recognition, CVPR 2005.
Level Set without Re-initialization

定义变分能量函数：
$$\epsilon (\phi) = \mu \mathcal{P}(\phi) + \lambda \mathcal{L}_g(\phi) + \nu \mathcal{A}_g(\phi)$$

其中有：
$$\left\{\begin{aligned}
    \mathcal{P}(\phi) &= \int_\Omega \frac{1}{2}(\left|\nabla \phi \right| - 1)^2 dxdy \\
    \mathcal{L}_g(\phi) &= \int_\Omega g \delta(\phi) \left|\nabla\phi\right| dxdy \\
    \mathcal{A}_g(\phi) &= \int_\Omega gH(-\phi) dxdy
\end{aligned}\right.$$

本次文主要分析对$\mathcal{L}_g(\phi)$和$\mathcal{A}_g(\phi)$的推导。

###### $\mathcal{L}_g(\phi)$项的推导
对于$\displaystyle{\mathcal{L}_g(\phi) = \int_\Omega g \delta(\phi) \left|\nabla\phi\right| dxdy}$，令：

$$F(\phi) = g \delta(\phi) \left|\nabla\phi\right|$$

令$h$为一个满足$h \Big|_{\partial\Omega}=0$的任意函数，令$\alpha$为一个小常数，有：

$$\begin{aligned}
    F(\phi + \alpha h) &= g \delta(\phi + \alpha h) \left|\nabla(\phi + \alpha h)\right| \\
    &= g \delta(\phi + \alpha h) \sqrt{ {(\phi+\alpha h)_x}^2 + {(\phi+\alpha h)_y}^2}
\end{aligned}$$

对其求偏导，有：

$$\begin{aligned}
    \frac{\partial F(\phi+\alpha h)}{\partial\alpha} &= g\left( \delta^{'}(\phi+\alpha h)\sqrt{ {(\phi+\alpha h)_x}^2 + {(\phi+\alpha h)_y}^2} + \delta(\phi + \alpha h)\frac{\nabla h \cdot \nabla(\phi+\alpha h)}{ \sqrt{ {(\phi+\alpha h)_x}^2 + {(\phi+\alpha h)_y}^2}} \right)
\end{aligned}$$

当$\alpha$足够小时，

$$\begin{aligned}
    \frac{\partial F(\phi+\alpha h)}{\partial\alpha} \bigg|_{\alpha \rightarrow 0} = g\left( \delta^{'}(\phi)\left| \nabla \phi \right| + \delta(\phi)\frac{h_x\phi_x + h_y\phi_y}{\left| \nabla \phi \right|} \right)
\end{aligned}$$

则有：

$$\begin{aligned}
    \frac{\partial \mathcal{L}_g(\phi+\alpha h)}{\partial\alpha} \bigg|_{\alpha \rightarrow 0} 
    &= \int_\Omega g \delta(\phi)\frac{h_x\phi_x + h_y\phi_y}{\left| \nabla \phi \right|} dxdy \\
    &= \int_\Omega \left( \frac{g\delta(\phi)\phi_x}{\left| \nabla \phi \right|} h_x \right) dxdy + \int_\Omega\left( \frac{g\delta(\phi)\phi_y}{\left| \nabla \phi \right|} h_y \right) dxdy \\
    &= \int_\Omega \left( 
        \frac{\partial}{\partial{x}}\left( \frac{g\delta(\phi)\phi_x}{\left| \nabla \phi \right|} h\right)
        - \frac{\partial}{\partial{x}}\left( \frac{g\delta(\phi)\phi_x}{\left| \nabla \phi \right|}\right)h 
        \right) dxdy 
    + \int_\Omega \left( 
        \frac{\partial}{\partial{y}}\left( \frac{g\delta(\phi)\phi_y}{\left| \nabla \phi \right|} h\right) - \frac{\partial}{\partial{y}}\left( \frac{g\delta(\phi)\phi_y}{\left| \nabla \phi \right|}\right)h 
    \right) dxdy \\
    &= \int_\Omega \left( 
        \frac{\partial}{\partial{x}}\left( \frac{g\delta(\phi)\phi_x}{\left| \nabla \phi \right|} h\right) 
        +\frac{\partial}{\partial{y}}\left( \frac{g\delta(\phi)\phi_y}{\left| \nabla \phi \right|} h\right)  \right) dxdy 
    -\int_\Omega \left( 
        \frac{\partial}{\partial{x}}\left( \frac{g\delta(\phi)\phi_x}{\left| \nabla \phi \right|}\right)h 
        +\frac{\partial}{\partial{y}}\left( \frac{g\delta(\phi)\phi_y}{\left| \nabla \phi \right|}\right)h 
    \right) dxdy 
\end{aligned}$$

对于左边项，由格林公式$\displaystyle{\oint_{\partial{\Omega}}Rdx+Sdy=\iint_{\Omega}\left( \frac{dS}{dx} - \frac{dR}{dy} \right)dxdy}$，且$h$满足$h \Big|_{\partial\Omega}=0$，有：

$$\begin{aligned}
    &\int_\Omega \left( 
        \frac{\partial}{\partial{x}}\left( \frac{g\delta(\phi)\phi_x}{\left| \nabla \phi \right|} h\right) 
        +\frac{\partial}{\partial{y}}\left( \frac{g\delta(\phi)\phi_y}{\left| \nabla \phi \right|} h\right)  \right) dxdy 
        = \oint_{\partial{\Omega}} h\left( \frac{g\delta(\phi)\phi_x}{\left| \nabla \phi \right|}dx - \frac{g\delta(\phi)\phi_y}{\left| \nabla \phi \right|}dy \right) = 0
\end{aligned}$$

因此
$$\begin{aligned}
    \frac{\partial \mathcal{L}_g(\phi+\alpha h)}{\partial\alpha} \bigg|_{\alpha \rightarrow 0} 
    &= -\int_\Omega h \left( 
        \frac{\partial}{\partial{x}}\left( \frac{g\delta(\phi)\phi_x}{\left| \nabla \phi \right|}\right)
        + \frac{\partial}{\partial{y}} \left(\frac{g\delta(\phi)\phi_y}{\left| \nabla \phi \right|}\right)
    \right) dxdy \\
    &= -\int_\Omega h \left( 
        \nabla \cdot (g\delta(\phi) \frac{\nabla\phi}{\left| \nabla \phi \right|})
    \right) dxdy
\end{aligned}$$

当$\mathcal{L}_g(\phi)$取到最小时，有：

$$\frac{\partial \mathcal{L}_g(\phi+\alpha h)}{\partial\alpha} \bigg|_{\alpha \rightarrow 0} 
= -\int_\Omega h \left(\nabla \cdot (g\delta(\phi) \frac{\nabla\phi}{\left| \nabla \phi \right|})\right) dxdy 
= 0
$$

由于函数$h$是任意的，必须有：

$$\nabla \cdot (g\delta(\phi) \frac{\nabla\phi}{\left| \nabla \phi \right|}) = 0$$

###### $\mathcal{A}_g(\phi)$项的推导

对于$\displaystyle{\mathcal{A}_g(\phi) = \int_\Omega gH(-\phi) dxdy}$，同理令：

$$F(\phi) = gH(-\phi)$$

有：

$$F(\phi+\alpha h) = gH[-(\phi+\alpha h)]$$

$$\begin{aligned}
    \frac{\partial F(\phi+\alpha h)}{\partial\alpha} \bigg|_{\alpha \rightarrow 0} 
    &= -gh \delta[-(\phi+\alpha h)] \bigg|_{\alpha \rightarrow 0} \\
    &= -gh \delta(-\phi)
\end{aligned}$$

有：

$$\begin{aligned}
    \frac{\partial \mathcal{A}_g(\phi+\alpha h)}{\partial\alpha} \bigg|_{\alpha \rightarrow 0} 
    &= - \int_\Omega h \Big( g \delta(\phi) \Big) dxdy
\end{aligned}$$

当$\mathcal{A}_g(\phi)$最小，同理有：

$$g \delta(\phi) = 0$$

###### 演化方程
要使变分能量函数
$$\epsilon (\phi) = \mu \mathcal{P}(\phi) + \lambda \mathcal{L}_g(\phi) + \nu \mathcal{A}_g(\phi)$$

最小化，即求
$$\phi^* = \argmin_\phi \epsilon (\phi)$$

通过上述的推导，可以得到梯度下降法的更新公式：

$$\begin{aligned}
    \frac{\partial\phi}{\partial t}
    = \mu\left(\nabla\phi - \nabla\cdot(\frac{\nabla\phi}{\left|\nabla\phi\right|}) \right)
    + \lambda\nabla \cdot (g\delta(\phi) \frac{\nabla\phi}{\left| \nabla \phi \right|})
    + \nu g \delta(\phi)
\end{aligned}$$

## 图像回复