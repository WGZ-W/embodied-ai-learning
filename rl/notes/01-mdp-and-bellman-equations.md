# 01 MDP 与 Bellman 方程

## 1. Markov 决策过程

MDP 定义为五元组 $(\mathcal S,\mathcal A,P,R,\gamma)$：状态、动作、转移、奖励和折扣因子。

Markov 性：

$$
p(s_{t+1}\mid s_{0:t},a_{0:t})=p(s_{t+1}\mid s_t,a_t).
$$

它不是说历史无用，而是状态已经包含预测未来所需的信息。若观测不满足这一条件，应考虑 POMDP、状态估计或历史编码器。

## 2. 策略与轨迹

随机策略 $\pi(a\mid s)$ 给出状态下动作分布。轨迹概率：

$$
p(\tau)=p(s_0)\prod_t\pi(a_t\mid s_t)p(s_{t+1}\mid s_t,a_t).
$$

策略改变动作分布，也会改变未来访问到的状态分布。

## 3. 奖励与回报

$$
G_t=r_{t+1}+\gamma r_{t+2}+\gamma^2r_{t+3}+\cdots.
$$

$\gamma<1$ 保证持续任务回报有限，也表达远期奖励权重。有限时域任务可使用 $\gamma=1$，但需正确处理终止。

奖励函数定义优化目标。奖励塑形不当可能产生 reward hacking；终止奖励、每步惩罚和折扣共同决定实际偏好。

## 4. 价值函数

$$
V^\pi(s)=E_\pi[G_t\mid s_t=s],
$$

$$
Q^\pi(s,a)=E_\pi[G_t\mid s_t=s,a_t=a].
$$

优势函数：

$$
A^\pi(s,a)=Q^\pi(s,a)-V^\pi(s).
$$

它衡量动作相对于当前策略平均行为有多好。

## 5. Bellman 期望方程

$$
V^\pi(s)=\sum_a\pi(a\mid s)
\sum_{s',r}p(s',r\mid s,a)[r+\gamma V^\pi(s')].
$$

Bellman 方程把长期回报递归分解为一步奖励和下一状态价值，是 DP、TD 和 Q-learning 的共同基础。

## 6. Bellman 最优方程

$$
V^*(s)=\max_a\sum_{s',r}p(s',r\mid s,a)
[r+\gamma V^*(s')].
$$

若已知 $Q^*$，选择 $\arg\max_aQ^*(s,a)$ 即得到确定性贪心最优策略。

## 7. 终止与截断

- Termination：进入任务定义的终止状态，后续价值通常为零。
- Truncation：因时间上限或外部条件停止，真实过程可能仍会继续，通常仍需 bootstrap。

把截断当终止会产生系统性价值偏差。

## 8. POMDP

POMDP 中智能体只获得观测 $o_t$。理想充分状态是 belief：

$$
b_t(s)=p(s_t=s\mid o_{1:t},a_{1:t-1}).
$$

实践中可使用滤波器、帧堆叠、RNN 或 Transformer 近似历史信息。

## 自测

1. 状态与观测有什么区别？
2. 为什么优势函数均值在策略动作分布下为零？
3. 截断样本为什么通常仍需 bootstrap？
4. 奖励塑形如何改变最优策略？

[上一层](../README.md) · [下一节](02-dynamic-programming.md)
