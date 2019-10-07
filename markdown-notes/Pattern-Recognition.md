# 模式识别
## 线性判别函数
线性判别是使用一个线性函数将一类样本进行**二分类**的问题。

线性判别函数可以将一系列样本划分为为$\omega_1$和$\omega_2$两类。记一个n维空间中**模式向量**（样本）为$x=[x_1,x_2,\cdots,x_n]^T$，**权向量**$w=[w_1,w_2,\cdots,w_n]^T$，有线性判别函数$d(x)=w_1x_1 + w_2x_2 + \cdots +w_nx_n + w_{n+1}$。为了方便将最后的这个常数项$w_{n+1}$也写成统一的形式，可以将模式向量和权向量写成增广形式，即

**增广模式向量**：

$$x=[x_1,x_2,\cdots,x_n,1]^T$$

**增广权向量**：

$$w=[w_1,w_2,\cdots,w_n, w_{n+1}]^T$$

那么有：

$$\displaystyle{
    d(x) = w^Tx 
    \left\{\begin{aligned}
    &> 0, \, x \in \omega_1\\
    &\leq 0,\, x \in \omega_2
    \end{aligned}\right.
}$$

### 使用线性判别函数进行多类分类
单个线性判别函数只能处理二分类问题，而对于多分类问题
#### 方法一：$\omega_i/\bar{\omega_i}$二分类法
$$\displaystyle{
    d_i(x) = w^Tx 
    \left\{\begin{aligned}
    &> 0, \, x \in \omega_i\\
    &\leq 0,\, x \in \bar{\omega_i}
    \end{aligned}\right.
}$$

#### 方法二：$\omega_i/\omega_j$二分类法

#### 方法三：$\omega_i/\bar{\omega_i}$二分类法
## 感知器算法（Percetion Approach）
感知器是通过对已知样本的训练和学习来求得线性判别函数系数的方法。我们知道对于一个模式向量$x$，应该有：

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

### 感知器算法修正权向量的几何意义

## 支持向量机（SVM, Support Vector Machine）
~~这个名字我十分想吐槽，第一次看到的时候以为是支持向量的机器，然而并没有什么关系。~~

### 点到超平面的距离

