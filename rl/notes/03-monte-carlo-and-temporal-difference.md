# 03 Monte Carlo 与 Temporal Difference

## 1. Monte Carlo 预测

MC 使用完整回报作为目标：

$$
V(S_t)\leftarrow V(S_t)+\alpha[G_t-V(S_t)].
$$

它不需要环境模型，也不 bootstrap，但必须等 episode 结束，目标方差较高。

First-visit MC 每回合只更新状态第一次访问；Every-visit MC 更新每次访问。二者在适当条件下都能收敛。

## 2. TD(0)

$$
\delta_t=R_{t+1}+\gamma V(S_{t+1})-V(S_t),
$$

$$
V(S_t)\leftarrow V(S_t)+\alpha\delta_t.
$$

TD 无需等回合结束，并以当前价值估计 bootstrap。目标偏差更大但方差通常更小。

## 3. n-step return

$$
G_t^{(n)}=R_{t+1}+\cdots+
\gamma^{n-1}R_{t+n}+\gamma^nV(S_{t+n}).
$$

$n=1$ 接近 TD，$n$ 到回合末端接近 MC。它提供偏差—方差折中。

## 4. Eligibility Trace 与 TD($\lambda$)

向后视角使用资格迹：

$$
e_t(s)=\gamma\lambda e_{t-1}(s)+\mathbf1(S_t=s),
$$

$$
V(s)\leftarrow V(s)+\alpha\delta_te_t(s).
$$

$\lambda$ 控制信用分配的时间范围。

## 5. SARSA

on-policy 更新：

$$
Q(S_t,A_t)\leftarrow Q(S_t,A_t)+
\alpha[R_{t+1}+\gamma Q(S_{t+1},A_{t+1})-Q(S_t,A_t)].
$$

它评估并改进实际执行的探索策略，因此学习到的行为会考虑探索风险。

## 6. Q-learning

off-policy 更新：

$$
Q(S_t,A_t)\leftarrow Q(S_t,A_t)+
\alpha[R_{t+1}+\gamma\max_aQ(S_{t+1},a)-Q(S_t,A_t)].
$$

行为策略可用 $\epsilon$-greedy 探索，目标策略是贪心策略。

## 7. 探索

$\epsilon$-greedy 简单但不利用动作不确定性。Softmax、UCB、Thompson Sampling 和内在奖励提供不同探索机制。

训练和评估必须分开：评估通常关闭探索噪声，并使用独立 episode。

## 8. Deadly Triad

函数逼近、bootstrapping 和 off-policy 同时出现时可能发散。DQN 的经验回放与目标网络正是缓解这一风险的工程设计。

## 自测

1. MC 与 TD 的偏差—方差如何不同？
2. SARSA 与 Q-learning 学习的目标策略有什么差异？
3. $n$ 和 $\lambda$ 如何影响信用分配？
4. Deadly Triad 包含哪三个因素？

[上一节](02-dynamic-programming.md) · [下一节](04-q-learning-and-dqn.md)
