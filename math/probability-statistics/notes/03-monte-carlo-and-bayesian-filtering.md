# 03 Monte Carlo、Bayes 滤波与 Kalman 滤波

## 1. Monte Carlo 估计

用独立样本近似期望：

$$
\hat I_N=\frac1N\sum_{i=1}^Nf(x_i),\qquad x_i\sim p.
$$

标准误差约随 $1/\sqrt N$ 缩小，要把误差减半通常需要约四倍样本。应报告多个独立重复实验或估计标准误差。

## 2. 重要性采样

$$
I=E_q\left[f(X)w(X)\right],
\quad w(x)=\frac{p(x)}{q(x)}.
$$

提议分布 $q$ 必须覆盖目标分布的重要区域。少数极大权重会导致有效样本量很低：

$$
N_{eff}\approx\frac{(\sum_iw_i)^2}{\sum_iw_i^2}.
$$

## 3. 状态空间模型

$$
x_t\sim p(x_t\mid x_{t-1},u_t),
\qquad z_t\sim p(z_t\mid x_t).
$$

Markov 假设认为给定当前状态后，未来与更早历史条件独立。状态必须包含满足该假设所需的信息。

## 4. Bayes 滤波递推

预测把上一时刻后验通过运动模型传播：

$$
\bar{bel}(x_t)=\int p(x_t\mid x_{t-1},u_t)bel(x_{t-1})dx_{t-1}.
$$

更新用观测似然修正：

$$
bel(x_t)=\eta p(z_t\mid x_t)\bar{bel}(x_t).
$$

运动增加不确定性，信息观测通常减少不确定性。

## 5. Kalman 滤波

线性高斯模型：

$$
x_t=Ax_{t-1}+Bu_t+w_t,\quad w_t\sim N(0,Q),
$$

$$
z_t=Hx_t+v_t,\quad v_t\sim N(0,R).
$$

预测：

$$
\bar\mu=A\mu+Bu,
\qquad \bar\Sigma=A\Sigma A^T+Q.
$$

更新：

$$
S=H\bar\Sigma H^T+R,
\qquad K=\bar\Sigma H^TS^{-1},
$$

$$
\mu=\bar\mu+K(z-H\bar\mu),
\qquad \Sigma=(I-KH)\bar\Sigma.
$$

数值实现推荐 Joseph 形式更新协方差，以更好保持对称半正定。

## 6. 创新与一致性

创新 $r=z-H\bar\mu$ 应近似零均值，协方差为 $S$。持续偏置说明模型、标定或噪声假设可能错误。

归一化创新平方：

$$
r^TS^{-1}r
$$

可用于门控异常观测和检查滤波一致性。

## 7. EKF 与粒子滤波

EKF 对非线性运动和观测模型在当前均值处一阶线性化。强非线性、多峰后验或错误线性化会使结果不可靠。

粒子滤波用带权样本表示后验，能表达非 Gaussian 和多峰分布，但高维下样本效率差。重采样缓解权重退化，也会造成样本贫化。

## 8. 调试清单

- 坐标系、时间戳和单位一致
- $Q,R$ 与实际噪声匹配
- 协方差保持对称半正定
- 创新无长期偏置
- 不直接求矩阵逆
- 丢失观测时只执行预测

## 自测

1. Monte Carlo 误差减半需要多少倍样本？
2. Kalman 增益如何平衡预测与观测？
3. 为什么要检查创新序列？
4. 粒子滤波的权重退化和样本贫化分别是什么？

[上一节](02-estimation-and-multivariate-gaussian.md) · [练习](../exercises.md)
