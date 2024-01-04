---
layout: post
title: Stochastic Process Review
subtitle: 随机过程知识点复习
date: 2024-01-03
author: Rain Chen
catalog: true
tags:
  - StochasticProcess
  - Mathematics
  - 數學
  - 隨機
---

# Stochastic Process Review

## Problem Strings

### 1.

#### Description

一个有限状态的离散随机变量平方之后，新随机变量的概率密度应该如何计算？

#### Solutions



### 2.

#### Description

已知两个联合正态分布的零均值随机变量 $X$，$Y$，他们的二维联合概率密度函数为 $f_{XY}(x, y) = \frac{1}{2 \pi A}\exp\{-(\frac{x^2}{6} - \frac{xy}{9} + \frac{y^2}{13.5})\}$，那么随机变量 $X$ 和 $Y$ 的相关系数是多少？二维联合概率密度中的常数 $A$ 是多少？

#### Solutions

由于两个联合正态分布的随机变量是零均值的，因此

$$
E(X) = E(Y) = m_X = m_Y = 0
$$

又根据随机变量联合正态的定义

$$
f_{XY}(x, y) = \frac{1}{2 \pi \sigma_{X1} \sigma_{Y} \sqrt{1 - r^2}} \exp\{-\frac{1}{2(1 - r^2)} [\frac{(x-m_X)^2}{\sigma_{X}^2} - \frac{2r(x-m_X)(y-m_Y)}{\sigma_{X} \sigma_{Y}} + \frac{(y-m_Y)^2}{\sigma_{Y}^2}] \}
$$

因此可以知道题目描述中的常数 $A$ 就是 $\sigma_X \sigma_Y \sqrt{1 - r^2}$。所以由此推断：

$$
\sigma_X^2 = 6, \sigma_X \sigma_Y = 9, \sigma_Y^2 = 13.5
$$

以及

$$
-\frac{1}{2(1-r^2)} = -1
$$

解得：

$$
r = \frac{\sqrt{2}}{2}, \sigma_X = \sqrt{6}, \sigma_Y = 3 \sqrt{\frac{3}{2}}
$$

所以相关系数 $r = \frac{\sqrt{2}}{2}$，常数 $A = \frac{9 \sqrt{2}}{2}$。

### 3.

#### Description

一个实随机过程的两个时刻相隔越远，则这两个时刻的自相关函数的绝对值是不是越小？

#### Solutions

是。自相关函数 $R_X(t_1, t_2) = \sigma_X^2(t) + m_X^2(t)$ 可正可负，其绝对值越大，表示相关性越强，一般来说，$t_1$、$t_2$ 相隔越远，相关性就越弱，$R_X(t_1, t_2)$ 的绝对值就越小；当 $t_1 = t_2 = 0$ 时，相关性是最强的，此时 $R_X(t_1, t_2)$ 最大。

### 4.

#### Description

对于一个已知的随机过程 $X(t)$，给定任意实数 $x$，根据 $X(t)$ 的值定义另一个随机过程 $Y(t)$：当 $X(t) \le x$ 时 $Y(t) = 1$，当 $X(t) \gt x$ 时 $Y(t) = 0$，那么 $Y(t)$ 的均值函数是什么样的？$Y(t)$ 的自相关函数是怎么样的？

#### Solutions

根据题目描述，可以得到下面这个表达式：

$$
Y(t) = \left\{
  \begin{array}{ll}
      1, X(t) \le x \\
      0, X(t) \gt x
  \end{array}
\right.
$$

$Y(t)$ 的均值函数是

$$
E[Y(t)] = 1 \times P\{Y(t) = 1\} + 0 \times P\{Y(t) = 0\}
$$

$$
= P\{X(t) \le x\} = F(x, t)
$$

是 $X(t)$ 的一维分布函数。

$Y(t)$ 的自相关函数

$$
R_Y(t_1, t_2) = E[Y(t_1Y(t_2))] = 1 \times 1 \times P\{Y(t_1) = 1, Y(t_2) = 1\} + 1 \times 0 \times P\{Y(t_1) = 1, Y(t_2) = 0\}
$$

$$
+ 0 \times 1 \times P\{Y(t_1) = 0, Y(t_2) = 1\} + 0 \times 0 \times P\{Y(t_1) = 0, Y(t_2) = 0\}
$$

$$
= F_X(x, x; t_1, t_2)
$$

是 $X(t)$ 的二维分布函数。

### 5.

#### Description

设随机过程 $Z(t) = X \cos t + Y \sin t, -\infty \lt t \lt \infty$，其中 $X$ 和 $Y$ 为相互独立的随机变量，并分别以概率 $\frac{2}{3}$、$\frac{1}{3}$ 取值 $-1$ 和 $2$，那么 $Z(t)$ 是广义平稳随机过程还是狭义平稳随机过程？

#### Solutions

狭义平稳随机过程：随机过程 $X(t)$ 的任意 $N$ 维分布不随时间起点的不同而变化。特别是一维概率密度 $f_X(x, t) = f_X(x)$ 与时间无关，二维概率密度 $f_X(x_1, x_2, t_1, t_2) = f_X(x_1, x_2, \tau)$ 仅与时间差有关，$(\tau = t_1 - t_2)$。

广义平稳随机过程：随机过程 $X(t)$ 的均值为常数，自相关函数只与 $\tau = t_1 - t_2$有关：

$$
m_X(t) = m_X
$$

$$
R_X(t_1, t_2) = R_X(\tau), (\tau = t_1 - t_2)
$$

| 取值 | $-1$ | $2$ |
| --- | --- | --- |
| $P$ | $\frac{2}{3}$ | $\frac{1}{3}$ |

所以可以计算 $X$ 和 $Y$ 的一阶矩和二阶矩：

$$
E(X) = -1 \times \frac{2}{3} + 2 \times \frac{1}{3} = 0
$$

$$
E(X^2) = (-1)^2 \times \frac{2}{3} + 2^2 \times \frac{1}{3} = 2
$$

同理，

$$
E(Y) = -1 \times \frac{2}{3} + 2 \times \frac{1}{3} = 0
$$

$$
E(Y^2) = (-1)^2 \times \frac{2}{3} + 2^2 \times \frac{1}{3} = 2
$$

所以可以求出

$$
m_Z(t) = E[X \sin t + Y \cos t] = E(X) \cos t + E(Y) \sin t = 0
$$

然后求自相关函数：

$$
R_Z(t_1, t_2) = E[Z(t_1)Z(t_2)] = E[(X \cos t_1 + Y \sin t_1)(X \cos t_2 + Y \sin t_2)]
$$

$$
= E(X^2) \cos t_1 \cos t_2 + E(Y^2) \sin t_1 \sin t_2 + E(XY)(\cos t_1 \sin t_2 + \sin t_1 \cos t_2)
$$

$$
= 2 [\cos t_1 \cos t_2 + \sin t_1 \sin t_2] = 2 \cos (t_2 - t_1)
$$

$$
= R_Z(t_2 - t_1)
$$

自相关函数只与 $\tau$ 有关，所以 $Z(t)$ 是广义平稳随机过程。又由于 $Z(t)$ 的一维分布函数与 $t$ 有关，所以 $Z(t)$ 不是狭义严格平稳过程（稍后补充证明过程）。

### 6.

#### Description

平稳随机过程的相关时间大小与随机过程的变化快慢的对应关系是什么样的？

#### Solutions

对于平稳随机过程来说，它的均值为常数，自相关函数只与时间的差值有关。为了比较随机过程的相关特性，引用「相关系数」的概念。相关系数实际上是对平稳随机过程的协方差函数作归一化处理：

$$
r_X(\tau) = \frac{C_X(\tau)}{\sigma_X^2} = \frac{R_X(\tau) - m_X^2}{\sigma_X^2}
$$

有时也被叫做**归一化相关函数**或者**标准协方差函数**。

对于相关时间，定义一个 $\tau_0$  参量表示。当 $\tau \rightarrow \infty$ 时，$r_X(\infty) = 0$，$X(t + \tau)$  与 $X(t)$ 才是不相关的；但是实际上当 $\tau$ 大到一定程度时，$r_X(\tau)$ 的值很小，可以近似看作不相关。因此当 $\tau \gt \tau_0$ 时，就认为 $X(t + \tau)$ 与 $X(t)$ 是不相关的。

相关时间越小，意味着相关系数随着 $\tau$ 的增大而迅速减小，说明随机过程随时间的变化快；反之，相关时间越大，则说明随机过程随时间的变化缓慢。

### 7.

#### Description

若平稳随机过程 $X(t)$ 的协方差函数 $K_X(\tau)$ 不满足 $K_X(\infty) = 0$，则该过程是否隐含周期性。

#### Solutions

协方差函数

$$
K_X(\tau) = K_X(t_1, t_2) = R_X(t_1, t_2) - m_X(t_1) m_X(t_2)
$$

由于 $X(t)$ 是平稳随机过程，均值 $m_X$ 为常数，所以 $R_X(t_1, t_2)$ 隐含周期性。由于相关函数隐含周期性，该随机过程也包含周期性。

### 8.

#### Description

一个随机过程 $X(t)$ 定义为 $X(t) = f(t + \epsilon)$，其中 $f(t)$ 是具有周期 $T$ 的周期信号，$\epsilon$ 是在区间 $[0, T]$ 内均匀分布的随机变量，请问 $X(t)$ 是不是平稳？

#### Solutions

判断一个随机过程是否平稳，则需要判断该随机过程的均值是否为一个常数，自相关函数是否只与时间差有关。

因此， $X(t)$ 的均值：

$$
E[X(t)] = E[f(t + \epsilon)]
$$

$$
= \int_{0}^{T}{f(t + \epsilon) \frac{1}{T}} \text{d} \epsilon
$$

$$
= \frac{1}{T} \int_{t}^{t + T}{f(x)} \text{d}x
$$

$$
= \frac{1}{T} \int_{0}^{T}{f(x)} \text{d}x = Constant.
$$

是一个常数；

$X(t)$ 的自相关函数：

$$
R_X(t + \tau, t) = E[X(t + \tau)X(t)] = E[f(t + \tau + \epsilon)X(t + \epsilon)]
$$

$$
= \int_{t}^{t + T}{f(x + \tau)f{t} \frac{1}{T}} \text{d}x
$$

$$
= \frac{1}{T} \int_{0}^{T}{f(t + \tau)f(t)} \text{d}x
$$

是时间差 $\tau$ 的函数，与其他参数无关。

因此 $X(t)$ 是一个平稳随机过程。

### 9.

#### Description

一个平稳随机过程叠加一个确定性的余弦信号之后，新过程是否为各态经历的随机过程？

#### Solutions

平稳随机过程 $X(t)$ 具有均值遍历性的充要条件是：

$$
\lim_{T \rightarrow \infty} \frac{1}{T} \int_{0}^{2T}{(1 - \frac{\tau}{2T})[R_X(\tau) - m_X^2]} \text{d}x = 0
$$

具有相关函数遍历性的充要条件是：

$$
\lim_{T \rightarrow \infty} \frac{1}{T} \int_{0}^{2T}{(1 - \frac{\tau}{2T})[R_\Phi(\tau) - R_X^2(\tau)]} \text{d}\tau = 0
$$

对于零均值的平稳正态随机过程，如果 $R_X(\tau)$ 连续，那么具有各态经历性的充要条件是：

$$
\int_{0}^{+\infty} \vert R_X(\tau)\vert \text{d}\tau < \infty
$$

由于平稳随机过程叠加了一个确定的余弦信号之后变得不平稳，因此新过程不是各态经历的随机过程。

### 10.

#### Description

已知 $X(n)$ 是一个独立随机过程，$Y(n)$ 的表达式为 $Y(n) = \sum_{k = 1}^{n}{X(k)}$，那么 $Y(n)$ 是独立随机过程吗？是增量随机过程吗？

#### Solutions



### 11.

#### Description

一个正态随机过程的均值为常数，自相关函数只与时间有关，该过程是否为一个狭义平稳随机过程？

#### Solutions

一个均值为常数并且自相关函数只与时间有关的**正态随机过程**是狭义（严格）平稳随机过程。严格平稳随机过程的均值与方差均是与时间无关的常数，自相关函数只与时间的差值有关，正态随机过程满足第一点，因此只要该正态随机过程的自相关函数只与时间差有关，它就是严格平稳随机过程。

### 12.

#### Description

对于一个平稳的正态随机过程，其全部的统计特性是否能够由其均值函数及自相关函数确定？

#### Solutions

是的，即是各态经历的。

### 13.

#### Description

如果正态平稳随机过程 $X(t)$ 的功率谱密度

$$
G_X(\omega) = \frac{6 \omega^2 + 12}{\omega ^ 4 + 5 \omega ^ 2 + 4}
$$

那么，$X(t)$ 的自相关函数是多少？能求出 $X(t)$ 的一维概率密度分布吗？

#### Solutions

根据**维纳-辛钦**定理，功率谱密度与自相关函数是一对傅立叶变换对。

$$
G_X(\omega) = \int_{-\infty}^{\infty}{e ^ {-\alpha \vert\tau\vert} e ^ {-j \omega \tau}} \text{d}\tau
$$

$$
= \int_{-\infty}^{0}{e^{\alpha\tau} e^{-j\omega\tau}}\text{d}\tau - \int_{0}^{+\infty}{e^{-\alpha\tau} e^{-j\omega\tau}}\text{d}\tau
$$

$$
= \frac{e^{(\alpha - j\omega)\tau}}{\alpha - j\omega}\Bigg\vert_{-\infty}^{0} - \frac{e^{-(\alpha + j\omega)}}{\alpha + j\omega}\Bigg\vert_{0}^{+\infty}
$$

$$
= \frac{1}{\alpha - j\omega} + \frac{1}{\alpha + j\omega}
$$

$$
= \frac{2 \alpha}{\alpha^2 + \omega^2}
$$


因此，可以对 $G_X(\omega)$ 进行因式分解：

$$
G_X(\omega) = \frac{6\omega^2 + 12}{\omega^4 + 5\omega^2 + 4} = \frac{6\omega^2 + 12}{(\omega^2 + 1)(\omega^2 + 4)} = \frac{2}{\omega^2 + 1} + \frac{4}{\omega^2 + 4}
$$

所以自相关函数：

$$
R_X(\tau) = \frac{\sqrt{2}}{1} e ^ {-\vert\tau\vert} + \frac{1}{2} e ^ {-2 \vert\tau\vert} = \sqrt{2} e ^ {-\vert\tau\vert} + \frac{1}{2} e ^ {-2 \vert\tau\vert}
$$

### 14.

#### Description

一个正态随机过程叠加一个确定性信号后服从什么分布？是否平稳？

#### Solutions

### 15.

#### Description

一个正态随机过程在某两个时刻上的采样值如果互不相关，则该过程在这两个时刻上的采样值是否一定相互独立？

#### Solutions

### 16.

#### Description

一个平稳过程 $X(t)$ 通过一个AWGN 信道到达接受端后会产生延迟 $T$，并且会叠加一个平稳的高斯白噪声 $W(t)$，则接收信号 $Y(t)$ 的表达式是什么？它与 $X(t)$ 的互相关函数是多少？

#### Solutions

### 17.

#### Description

希尔伯特变换器的幅频特性具有什么特点？是一个什么网络？它对输入信号的所有频率分量放大能力是一样的吗？

#### Solutions

### 18.

#### Description

对于一个实平稳随机过程和它的希尔伯特变换，二者之间的自相关函数具有什么关系？功率谱密度有什么关系？

#### Solutions

### 19.

#### Description

已知信号 $X(t)$ 的表达式为 $X(t) = (1 + A \cos \omega t)$，$\omega \ll \omega_0$，则 $X(t)$ 的复解析信号表达式是怎么样的？

#### Solutions

### 20.

#### Description

一个窄带正态随机过程的包络服从什么分布？相位服从什么分布？其包络和相位是否相互独立？

#### Solutions



### 21.

#### Description

若某通信系统中的一个调频信号 $X(t) = A \cos [\omega_0 t + m(t)]$ 为窄带信号，$A$ 为常数，则其希尔伯特变换是什么？其复信号表示是什么？

#### Solutions

求希尔伯特变换：

$$
\mathscr{H}[X(t)] = \hat{X}(t) = \frac{1}{\pi} \int_{-\infty}^{\infty}{\frac{x(\tau)}{t - \tau}} \text{d}\tau
$$

即是 $X(t)$ 与 $\frac{1}{\pi}$ 的卷积。因此，$X(t) = A \cos [\omega_0 t + m(t)]$ 的希尔伯特变换是

$$
\hat{X}(t) = A \sin [\omega_0 t + m(t)]
$$

复信号表示为：

$$
\tilde{X}(t) = X(t) + j \hat{X}(x)
$$

### 22.

#### Description

高斯白噪声中的「高斯」指的是什么？「白」指的是什么？它通过一个窄带系统之后是什么噪声？「窄带」指的是什么？

#### Solutions

窄带是指：$\omega_0$ 大于物理带宽的 $\frac{1}{2}$（中频）；高斯指的是正态随机过程，任意 $n$ 个随机变量服从 $n$ 维的联合正态分布；白噪声是指通带内功率谱密度是常数。

### 23.

#### Description

一个实平稳随机过程 $X(t)$ 的解析信号 $\tilde{X}(t)$ 的表达式是怎么样的？$E[\tilde{X}(t) \tilde{X^*}(t - \tau)]$ 和 $E[\tilde{X}(t) \tilde{X}(t - \tau)]$ 的结果一样吗？

#### Solutions

结论：结果不一样。

解析信号（复信号表示）：

$$
\tilde{X}(t) = X(t) + j \hat{X}(t)
$$

其中 $\hat{X}(t)$ 是 $X(t)$ 的希尔伯特变换。

- 求 $E[\tilde{X}(t) \tilde{X}^*(t - \tau)]$

$$
E[\tilde{X}(t) \tilde{X}^* (t - \tau)] = R_{\tilde{X}}(\tau) = 2 [R_X(\tau) + j R_X(\tau)]
$$

- 求 $E[\tilde{X}(t) \tilde{X}(t - \tau)]$

$$
E[\tilde{X}(t) \tilde{X}(t - \tau)] = E \{ [X(t) + j \hat{X}(t)] [X(t - \tau) + j \hat{X}(t - \tau)] \}
$$

$$
= R_X(\tau) + j R_{\hat{X}X}(\tau) + j R_{X \hat{X}}(\tau) - R_{\hat{X}(\tau)}
$$

根据希尔伯特变换性质 $R_X(\tau) = R_{\hat{X}}(\tau)$ 和 $R_{\hat{X}X}(\tau) = -R_{X \hat{X}}(\tau)$，可以得到：

$$
E[\tilde{X}(t) \tilde{X}(t - \tau)] = R_X(\tau) + j R_{\hat{X}X}(\tau) + j R_{X \hat{X}}(\tau) - R_{\hat{X}(\tau)} = 0
$$

### 24.

#### Description

一个齐次的马尔可夫链是否是一个平稳的随机过程？

#### Solutions

不一定是。

> 满足的条件和证明稍后补充。

### 25.

#### Description

一个马尔可夫链的状态空间 $I = \{ 1, 2, ..., N \}$，已知 $s$ 时刻到 $r$ 时刻及 $r$ 时刻到 $n$ 时刻的状态转移概率分别为

$$
p_{ik}(s, r) = P \{ X(r) = K \mid X(s) = i \}
$$

$$
p_{kj}(r, n) = P \{ X(n) = j \mid X(r) = k \}
$$

则

$$
P \{ X(n) = j \mid X(s) = i\}
$$

是多少？

#### Solutions



### 26.

#### Description

一个马尔可夫过程 $X(t)$，当 $t_s \lt t_r \lt t_n$ 时，如果 $t_r$ 时刻的状态 $X(t_r)$ 已知，那么随机变量 $X(t_s)$ 与 $X(t_n)$ 是否统计独立？用概率密度表示如何？

#### Solutions

### 27.

#### Description

泊松随机过程是否为独立增量随机过程？是否为独立随机过程？

#### Solutions

泊松随机过程是**独立增量**随机过程，即发生在不相交的时间区间中的事件的个数是彼此独立的；但泊松随机过程不是独立随机过程。

### 28.

#### Description

已知某泊松过程 $X(t)$ 的到达率参数为 $\lambda$。给定两个时刻 $s$ 和 $t(s \lt t)$，如果已知 $s$ 时刻的事件发生次数为 $k$，则概率

$$
P\{X(t) = n \mid X(s) = k \}
$$

表示什么含义？其值如何计算？

#### Solutions

条件概率。$P\{X(t) = n \mid X(s) = k \}$ 表示在「已知 $s$ 时刻事件发生次数为 $k$ 」的条件下 $t$ 时刻事件发生的次数。它反映了泊松过程的**独立增量性质**，也就是说泊松过程不同时间区间的事件数是相互独立的。

可以根据泊松分布的公式进行计算：

$$
P \{X(t) = n \mid X(s) = k \} = \frac{e^{-\lambda (t-s)} (\lambda (t-s))^{n-k}}{(n-k)!}, \quad n \geq k
$$

### 29.

#### Description

泊松随机过程的概率分布率如何计算？两个独立泊松过程叠加后服从什么分布？叠加后得到的新随机过程的强度是多少？

#### Solutions

泊松随机过程的概率分布：

$$
P\{ N(t) = k \} = \frac{(\lambda t)^k}{k!} \cdot e ^ {-\lambda t}
$$

分别设两个独立的泊松过程强度参数 $\lambda_1$ 和 $\lambda_2$，概率分布为 $N_1(t)$ 和 $N_2(t)$。那么叠加后：

$$
P \{N_1(t) + N_2(t) = k\} = \frac{(\lambda_1 t + \lambda_2 t)^k}{k!} \cdot e ^ {-(\lambda_1 + \lambda_2) t}
$$

因此叠加后的强度 $\lambda = \lambda_1 + \lambda_2$，仍然服从泊松分布。
