# 强化学习

强化学习研究智能体如何通过与环境交互、利用延迟奖励学习决策策略。本模块从 Markov 决策过程出发，覆盖价值学习、策略优化、连续控制、模型式方法、模仿学习和离线强化学习，并强调实验可靠性与具身任务中的闭环评估。

## 学习目标

完成本模块后，应能够：

- 将序贯决策问题形式化为 MDP 或 POMDP
- 推导 Bellman 期望方程和最优方程
- 实现动态规划、Monte Carlo、TD、SARSA 与 Q-learning
- 理解 DQN 的经验回放、目标网络及常见改进
- 推导策略梯度、Actor–Critic、GAE 与 PPO
- 理解 DDPG、TD3、SAC 等连续控制算法
- 区分模型式、模仿学习与离线强化学习的适用条件
- 设计可复现、可比较且面向闭环任务的 RL 实验

## 前置知识

- [线性代数](../math/linear-algebra/README.md)
- [微积分](../math/calculus/README.md)
- [概率统计](../math/probability-statistics/README.md)
- [最优化](../math/optimization/README.md)
- [机器学习与深度学习](../ml-dl/README.md)
- Python、NumPy 与 PyTorch 基础

## 内容索引

| 顺序 | 主题 | 关键问题 | 笔记 |
| --- | --- | --- | --- |
| 1 | MDP 与 Bellman 方程 | 如何数学化描述状态、动作与长期回报？ | [01-mdp-and-bellman-equations](notes/01-mdp-and-bellman-equations.md) |
| 2 | 动态规划 | 已知模型时如何求最优价值和策略？ | [02-dynamic-programming](notes/02-dynamic-programming.md) |
| 3 | Monte Carlo 与 TD | 未知模型时如何从经验估计价值？ | [03-monte-carlo-and-temporal-difference](notes/03-monte-carlo-and-temporal-difference.md) |
| 4 | Q-learning 与 DQN | 如何学习离散动作的最优控制？ | [04-q-learning-and-dqn](notes/04-q-learning-and-dqn.md) |
| 5 | 策略梯度与 Actor–Critic | 如何直接优化随机策略？ | [05-policy-gradient-and-actor-critic](notes/05-policy-gradient-and-actor-critic.md) |
| 6 | PPO 与 GAE | 如何稳定地执行多轮策略更新？ | [06-ppo-and-gae](notes/06-ppo-and-gae.md) |
| 7 | 连续控制 | DDPG、TD3 与 SAC 如何处理连续动作？ | [07-continuous-control](notes/07-continuous-control.md) |
| 8 | 模型式、模仿与离线 RL | 如何利用模型、演示或固定数据集？ | [08-model-based-imitation-and-offline-rl](notes/08-model-based-imitation-and-offline-rl.md) |
| 9 | 实验与具身应用 | 如何判断算法改进真实有效？ | [09-experimentation-and-embodied-rl](notes/09-experimentation-and-embodied-rl.md) |
| 10 | 练习与项目 | 是否能完成从表格算法到连续控制的闭环？ | [exercises](exercises.md) |

## 建议学习路线

| 阶段 | 内容 | 产出 |
| --- | --- | --- |
| 第 1 周 | MDP、Bellman 方程与动态规划 | 手算小型 MDP，实现 Value Iteration |
| 第 2 周 | Monte Carlo、TD、SARSA、Q-learning | 完成表格型控制实验 |
| 第 3 周 | DQN 与稳定训练技巧 | 训练离散控制任务并做消融 |
| 第 4 周 | 策略梯度、Actor–Critic、GAE、PPO | 从零实现最小 PPO |
| 第 5 周 | DDPG、TD3 与 SAC | 完成连续控制对比 |
| 第 6 周 | 模型式、模仿与离线 RL | 完成行为克隆或离线数据实验 |
| 第 7 周 | 实验复现与具身评估 | 多种子报告、失败分析与闭环测试 |

## 核心公式速查

| 概念 | 公式 |
| --- | --- |
| 折扣回报 | $G_t=\sum_{k=0}^{\infty}\gamma^kr_{t+k+1}$ |
| 状态价值 | $V^\pi(s)=\mathbb{E}_\pi[G_t\mid S_t=s]$ |
| 动作价值 | $Q^\pi(s,a)=\mathbb{E}_\pi[G_t\mid S_t=s,A_t=a]$ |
| Bellman 最优方程 | $Q^*(s,a)=\mathbb{E}[r+\gamma\max_{a'}Q^*(s',a')]$ |
| TD 误差 | $\delta_t=r_{t+1}+\gamma V(s_{t+1})-V(s_t)$ |
| 策略梯度 | $\nabla J=\mathbb{E}[\nabla\log\pi_\theta(a\mid s)A(s,a)]$ |
| PPO 比率 | $r_t(\theta)=\pi_\theta(a_t\mid s_t)/\pi_{old}(a_t\mid s_t)$ |

## 完成标准

- [ ] 能区分环境回报、价值函数、优势函数和 TD 误差
- [ ] 能解释 on-policy/off-policy、model-free/model-based 的差别
- [ ] 能从零实现表格型 Q-learning 和一个深度 RL 算法
- [ ] 能解释 DQN、PPO 和 SAC 的主要稳定化机制
- [ ] 能识别 bootstrapping、函数逼近和 off-policy 组合的风险
- [ ] 能进行多随机种子评估和公平基线比较
- [ ] 完成练习与至少一个综合项目

## 参考资源

- Sutton & Barto, *Reinforcement Learning: An Introduction*
- David Silver, Reinforcement Learning Course
- Berkeley CS285: Deep Reinforcement Learning
- OpenAI Spinning Up
- Stanford CS234: Reinforcement Learning

[返回仓库首页](../README.md)
