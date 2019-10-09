# 模式识别

- [模式识别](#模式识别)
  - [线性回归](#线性回归)
  - [对数几率回归（Logistic Regression）](#对数几率回归logistic-regression)
  - [Softmax](#softmax)
  - [线性判别函数](#线性判别函数)
    - [定义](#定义)
    - [求解线性判别函数的系数](#求解线性判别函数的系数)
      - [感知器算法（Percetron Approach）](#感知器算法percetron-approach)
        - [修正权向量的几何意义](#修正权向量的几何意义)
  - [多分类问题](#多分类问题)
    - [OvO(One vs. One)：$\omega_i/\bar{\omega_i}$二分类法](#ovoone-vs-oneomega_ibaromega_i二分类法)
    - [OvR(One vs. Rest)：$\omega_i/\omega_j$二分类法](#ovrone-vs-restomega_iomega_j二分类法)
      - [OvR改](#ovr改)
    - [MvM()](#mvm)
  - [神经网络](#神经网络)
    - [感知器](#感知器)
      - [XOR problem](#xor-problem)
  - [支持向量机（SVM, Support Vector Machine）](#支持向量机svm-support-vector-machine)
    - [线性SVM](#线性svm)
      - [点到超平面的距离](#点到超平面的距离)


## 线性回归
回归（Regression）是研究两组变量之间关系的统计分析方法。在线性回归中我们要做的就是通过一组（$m$个）已知样本$\displaystyle{\{ (\boldsymbol{x}_1,y_1), (\boldsymbol{x}_2,y_2), \cdots, (\boldsymbol{x}_m,y_m) \}}$求得线性模型的最佳（某种意义上的）参数估计值$(w^*,b^*)$，以尽可能准确的预测输出。

记一个$d$维空间中的某一个样本为$\boldsymbol{x}_i=[x_{i1},x_{i2},\cdots,x_{id}]^T$，样本共有$m$个，权向量$\boldsymbol{w}=[w_1,w_2,\cdots,w_d]^T$，线性回归模型为：

$$\displaystyle{\begin{aligned}
    f(x_i) &= w_1x_{i1} + w_2x_{i2} + \cdots +w_dx_{id} + b\\
    &= \boldsymbol{w}^T\boldsymbol{x}+b \simeq y_i
\end{aligned}}$$

参数估计值$(\boldsymbol{w}^*,b^*)$可以用最小二乘法（Least squared method），有均方误差$E$，求$(\boldsymbol{w}^*,b^*)$使其最小，即：

$$\displaystyle{
    E(\boldsymbol{w},b)=\sum_{i=1}^{m}(f(x_i)-y_i)^2
}$$

$$\displaystyle{
    (w^*,b^*) = \mathop{argmin}\limits_{\boldsymbol{w},b} E(\boldsymbol{w},b)
}$$

$\boldsymbol{w}^*$和$b^*$是有闭式（closed-form）解的。求解:

$$\displaystyle{\begin{aligned}
    \frac{\partial{E}}{\partial{w}}
\end{aligned}}$$

## 对数几率回归（Logistic Regression）
如果我们希望回归完成的不是$f:\boldsymbol{x_i} \rightarrow y_i$，而是与$y$相关的信息。比如如果有两类$\omega_+,\omega_-$，当$y_i>0$，$\boldsymbol{x_i} \in \omega_+$；当$y_i<0$，$\boldsymbol{x_i} \in \omega_-$。我们希望当$\boldsymbol{x_i} \in \omega_+$，$f(\boldsymbol{x}_i)=1$；当$\boldsymbol{x_i} \in \omega_-$，$f(\boldsymbol{x}_i)=0$（临界值任意判别），我们可以寻找一个新的函数使标签$z=g(y)=g(\boldsymbol{w}^T\boldsymbol{x}+b)$，$z \in \{0,1\}$。该模型称为广义线性模型，$g(\cdot)$称为联系函数。



对数几率回归[^1]

[^1]:译名来自周志华《机器学习》。Logistic Logit Logic

说是回归（Regression），实际上用作分类（Classification）。

## Softmax

## 线性判别函数
### 定义
线性判别函数是一个线性函数，可以将一类样本进行**二分类**。

线性判别函数可以将一系列样本划分为为$\omega_1$和$\omega_2$两类。记一个n维空间中**模式向量**（样本）为$\boldsymbol{x}=[x_1,x_2,\cdots,x_n]^T$，**权向量**$\boldsymbol{w}=[w_1,w_2,\cdots,w_n]^T$，有线性判别函数$d(x)=w_1x_1 + w_2x_2 + \cdots +w_nx_n + b$。

那么有：

$$\displaystyle{
    d(x) = \boldsymbol{w}^T\boldsymbol{x} +b
    \left\{\begin{aligned}
    &> 0, \, x \in \omega_1\\
    &\leq 0,\, x \in \omega_2
    \end{aligned}\right.
}$$

### 求解线性判别函数的系数
#### 感知器算法（Percetron Approach）
~~我也不知道课程PPT上为什么这里叫做感知器算法，尽管他还没有发展到真正意义上的感知器（Percetron）。~~
为了方便将最后的这个常数项$b$也写成统一的形式，可以将模式向量和权向量写成增广形式，即
**增广模式向量**：

$$x=[x_1,x_2,\cdots,x_n,1]^T$$

**增广权向量**：
$$w=[w_1,w_2,\cdots,w_n, b]^T$$
感知器算法是通过对已知样本的训练和学习来求得线性判别函数系数的方法。我们知道对于一个模式向量$x$，应该有：

$$\displaystyle{
    d(x) = w^Tx 
    \left\{\begin{aligned}
    &> 0, \, x \in \omega_1\\
    &\leq 0,\, x \in \omega_2
    \end{aligned}\right.
}$$

现在对于训练样本集中的样本$x_k$进行训练求解权向量。记$w_{(i)}$为第$i$步迭代时的权向量，若$x_k \in \omega_1$，但$w_{(i)}^T x_k \leq 0$，说明分类器分类错误，下一步迭代中的权向量应修正为：

$$w_{(i+1)} = w_{(i)} + Cx_k$$

$C$为校正增量。

若$x_k \in \omega_2$，但$w_{(i)}^T x_k > 0$，同样分类错误，权向量应修正为：

$$w_{(i+1)} = w_{(i)} - Cx_k$$

若分类正确，则权向量保持不变，为：

$$w_{(i+1)} = w_{(i)}$$

-----------

现在对$x_k^{'} \in \omega_2$的样本乘以$(-1)$，有$w_{(i)}^T(-x_k^{'}) < 0$时分类错误，此时权向量修正为：

$$w_{(i+1)} = w_{(i)} + C(-x_k^{'})$$

令$x_k=-x_k^{'}$，可以得到统一的感知器算法表达式：

$$\displaystyle{
    w_{(i+1)} =
    \left\{\begin{aligned}
    &w_{(i)} &, &w_{(i)}^Tx_k > 0\\
    w_{(i)} &+ Cx_k &, &w_{(i)}^Tx_k \leq 0
    \end{aligned}\right.
}$$

##### 修正权向量的几何意义
## 多分类问题
单个线性判别函数只能处理二分类问题，而对于多分类问题，则使用多个线性判别函数进行处理。若共有$M$个不同类别$\omega_1,\omega_2,\cdots,\omega_M$，分类方法有：

### OvO(One vs. One)：$\omega_i/\bar{\omega_i}$二分类法

$$\displaystyle{
    d_i(x) = w^Tx 
    \left\{\begin{aligned}
    &> 0, \, x \in \omega_i\\
    &\leq 0,\, x \in \bar{\omega_i}
    \end{aligned}\right. ,\, i=1,2,\cdots,M
}$$

### OvR(One vs. Rest)：$\omega_i/\omega_j$二分类法

#### OvR改

### MvM()
ECOC

## 神经网络
### 感知器
#### XOR problem
## 支持向量机（SVM, Support Vector Machine）
~~这个名字我十分想吐槽，第一次看到的时候以为是支持向量的机器，然而并没有什么关系。~~
### 线性SVM
#### 点到超平面的距离

