# 05 策略梯度与 Actor–Critic

## 1. 直接优化策略

参数化策略 $\pi_\theta(a\mid s)$，目标：

$$
J(\theta)=E_{\tau\sim\pi_\theta}[G(\tau)].
$$

Likelihood-ratio 技巧：

$$
\nabla_\theta\log p_\theta(\tau)
=\sum_t\nabla_\theta\log\pi_\theta(a_t\mid s_t),
$$

环境转移概率不依赖策略参数，因此无需对环境求导。

## 2. REINFORCE

$$
\nabla J=E\left[\sum_t
\nabla\log\pi_\theta(a_t\mid s_t)G_t\right].
$$

MC 回报无偏但方差高。减去只依赖状态的 baseline 不改变期望：

$$
G_t-b(s_t).
$$

最常用 baseline 是 $V^\pi(s)$，差值即优势估计。

## 3. Actor–Critic

Actor 更新策略，Critic 估计价值。一步优势可用 TD error：

$$
\hat A_t=\delta_t=r_t+\gamma V_\phi(s_{t+1})-V_\phi(s_t).
$$

Actor loss 常写为：

$$
L_{actor}=-E[\log\pi_\theta(a_t\mid s_t)\hat A_t].
$$

Critic 用回归损失拟合 value target。

## 4. 熵正则

$$
L=L_{actor}-\beta E[\mathcal H(\pi(\cdot\mid s))].
$$

熵奖励抑制策略过早坍缩，但系数过大可能导致长期随机。应同时监控策略熵与任务回报。

## 5. 连续动作策略

常用对角 Gaussian：策略网络输出 $\mu(s)$ 和 $\log\sigma(s)$。训练使用采样动作，评估可使用均值动作。

动作经过 `tanh` 限幅时，log-probability 必须加入变量变换修正；否则梯度目标错误。

## 6. On-policy 数据

策略梯度通常依赖当前策略分布。策略更新后，旧轨迹不再严格 on-policy；可用重要性采样校正，但权重方差可能很大。

## 7. 常见问题

- 忘记对优势停止梯度
- 优势尺度过大导致更新不稳定
- Critic 过拟合或滞后
- 动作 log-prob 维度未正确求和
- 终止和截断 bootstrap 处理错误

## 自测

1. 为什么策略梯度不需要环境可微？
2. baseline 为什么不改变梯度期望？
3. Actor 与 Critic 分别学习什么？
4. `tanh` 动作为什么需要 log-prob 修正？

[上一节](04-q-learning-and-dqn.md) · [下一节](06-ppo-and-gae.md)
