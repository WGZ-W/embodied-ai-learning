# 概率统计

概率用于描述不确定性，统计用于从有限数据推断未知规律。具身智能中的传感器噪声、状态估计、学习算法和实验评价都依赖概率统计。

## 学习目标

- 掌握条件概率、独立性和 Bayes 公式
- 理解随机变量、常见分布、期望与协方差
- 掌握最大似然、最大后验和置信区间
- 理解多元高斯、变量变换和 Monte Carlo 方法
- 能推导 Bayes 滤波与 Kalman 滤波的基本更新

## 专题笔记

| 顺序 | 主题 | 笔记 |
| --- | --- | --- |
| 1 | 概率规则、随机变量与常见分布 | [01-probability-and-distributions](notes/01-probability-and-distributions.md) |
| 2 | 多元高斯、估计与统计推断 | [02-estimation-and-multivariate-gaussian](notes/02-estimation-and-multivariate-gaussian.md) |
| 3 | Monte Carlo、Bayes 滤波与 Kalman 滤波 | [03-monte-carlo-and-bayesian-filtering](notes/03-monte-carlo-and-bayesian-filtering.md) |

## 1. 概率基础

条件概率与乘法规则：

$$
P(A\mid B)=\frac{P(A\cap B)}{P(B)},\qquad
P(A\cap B)=P(A\mid B)P(B).
$$

Bayes 公式：

$$
P(A\mid B)=\frac{P(B\mid A)P(A)}{P(B)}.
$$

全概率公式把边缘概率写成互斥情形之和。条件独立不等于独立；图模型常利用条件独立分解复杂联合分布。

## 2. 随机变量与分布

离散变量用概率质量函数，连续变量用概率密度函数。密度值可以大于 1，但积分必须为 1。

常见分布：

| 分布 | 参数 | 常见用途 |
| --- | --- | --- |
| Bernoulli | $p$ | 成功/失败事件 |
| Categorical | $p_1,\ldots,p_K$ | 多类别结果 |
| Binomial | $n,p$ | 多次独立试验成功数 |
| Gaussian | $\mu,\sigma^2$ | 连续噪声与误差 |
| Poisson | $\lambda$ | 单位区间事件计数 |
| Beta | $\alpha,\beta$ | Bernoulli 概率先验 |

## 3. 期望、方差与协方差

$$
\mathbb{E}[X]=\int xp(x)\,dx,
\qquad
\operatorname{Var}(X)=\mathbb{E}[(X-\mu)^2].
$$

协方差：

$$
\operatorname{Cov}(X,Y)=
\mathbb{E}[(X-\mu_X)(Y-\mu_Y)].
$$

零协方差一般不代表独立；联合高斯情况下例外。相关系数将协方差归一化到 $[-1,1]$。

线性变换的均值与协方差：

$$
\mathbf{y}=A\mathbf{x}+\mathbf{b},
$$

$$
\mathbb{E}[\mathbf{y}]=A\boldsymbol\mu+\mathbf{b},
\qquad
\Sigma_y=A\Sigma_xA^T.
$$

## 4. 多元高斯

$$
p(\mathbf{x})=\frac{1}{(2\pi)^{d/2}|\Sigma|^{1/2}}
\exp\left[-\frac12(\mathbf{x}-\mu)^T\Sigma^{-1}(\mathbf{x}-\mu)\right].
$$

协方差决定不确定性椭球的方向和尺度，精度矩阵 $\Lambda=\Sigma^{-1}$ 常用于信息形式推导。实际计算对数似然时应使用 Cholesky 分解而非显式求逆和行列式。

## 5. 参数估计

最大似然估计：

$$
\hat\theta_{MLE}=\arg\max_\theta p(\mathcal{D}\mid\theta).
$$

通常最大化对数似然。最大后验估计加入先验：

$$
\hat\theta_{MAP}=\arg\max_\theta
[\log p(\mathcal{D}\mid\theta)+\log p(\theta)].
$$

从优化视角，负对数先验常对应正则化项，例如高斯先验对应 $L_2$ 正则化。

## 6. 统计推断

- 点估计给出单个参数值。
- 置信区间描述重复采样意义下估计程序的覆盖率。
- 假设检验衡量数据与原假设的相容程度。
- $p$ 值不是原假设为真的概率，也不衡量效应大小。

实验报告应同时给出效应量、不确定性、样本量和数据生成过程，避免只报告单次最好结果。

## 7. Monte Carlo

若 $x_i\sim p(x)$，则：

$$
\mathbb{E}_p[f(X)]\approx\frac1N\sum_{i=1}^Nf(x_i).
$$

样本均值误差通常按 $O(N^{-1/2})$ 缩小。重要性采样使用易采样分布 $q$：

$$
\mathbb{E}_p[f(X)]
=\mathbb{E}_q\left[f(X)\frac{p(X)}{q(X)}\right].
$$

$q$ 必须覆盖 $p$ 的重要区域，否则权重方差可能极大。

## 8. Bayes 滤波

预测：

$$
p(x_t\mid z_{1:t-1})=
\int p(x_t\mid x_{t-1})p(x_{t-1}\mid z_{1:t-1})dx_{t-1}.
$$

更新：

$$
p(x_t\mid z_{1:t})\propto
p(z_t\mid x_t)p(x_t\mid z_{1:t-1}).
$$

线性高斯假设下得到 Kalman 滤波；非线性或非高斯问题可使用 EKF、UKF、粒子滤波等近似方法。

## 完成标准

- [ ] 能区分联合、边缘和条件概率
- [ ] 能计算期望、方差、协方差和线性误差传播
- [ ] 能推导常见模型的 MLE/MAP
- [ ] 能正确解释置信区间和 $p$ 值
- [ ] 能写出 Bayes 滤波的预测与更新步骤
- [ ] 完成[练习与自测](exercises.md)

## 参考资源

- *Introduction to Probability* — Blitzstein, Hwang
- *Pattern Recognition and Machine Learning* — Christopher Bishop
- *Probabilistic Robotics* — Thrun, Burgard, Fox
- Harvard Stat 110

[返回数学模块](../README.md)
