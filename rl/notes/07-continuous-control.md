# 07 连续控制：DDPG、TD3 与 SAC

## 1. 确定性策略梯度

DDPG 使用确定性 Actor $a=\mu_\theta(s)$ 和 Critic $Q_\phi(s,a)$：

$$
\nabla_\theta J\approx E_s
[\nabla_aQ_\phi(s,a)|_{a=\mu(s)}\nabla_\theta\mu_\theta(s)].
$$

它是 off-policy 算法，使用 Replay Buffer 和目标网络。探索通过给动作添加噪声完成。

## 2. Critic 目标

$$
y=r+\gamma(1-d)Q_{\bar\phi}
(s',\mu_{\bar\theta}(s')).
$$

Critic 误差会通过 Actor 放大，因此 DDPG 对超参数和 Q 过估计敏感。

## 3. TD3

TD3 的三个关键改进：

1. Twin Critics：目标使用两个 Q 的较小值
2. Delayed Policy Update：Critic 多次更新后再更新 Actor
3. Target Policy Smoothing：目标动作加入裁剪噪声

这些设计共同降低过估计和对狭窄 Q 峰值的利用。

## 4. 最大熵 RL

SAC 优化奖励与熵：

$$
J(\pi)=E\left[\sum_t\gamma^t
(r_t+\alpha\mathcal H(\pi(\cdot\mid s_t)))\right].
$$

随机策略鼓励探索并能表达多种近优动作。

## 5. SAC 更新

Soft value target：

$$
y=r+\gamma(1-d)
[\min_iQ_{\bar\phi_i}(s',a')-\alpha\log\pi(a'\mid s')].
$$

Actor 通过重参数技巧优化：

$$
J_\pi=E[\alpha\log\pi(a\mid s)-\min_iQ_{\phi_i}(s,a)].
$$

温度 $\alpha$ 可自动调节，使策略熵接近目标熵。

## 6. 动作缩放

网络输出通常经 `tanh` 映射到 $[-1,1]$，再缩放到环境动作范围。训练、buffer 存储和环境接口必须明确动作处于哪个尺度。

## 7. 比较

| 算法 | 策略 | 探索 | 主要特点 |
| --- | --- | --- | --- |
| DDPG | 确定性 | 外加噪声 | 简单但敏感 |
| TD3 | 确定性 | 外加噪声 | 降低 Q 过估计 |
| SAC | 随机 | 策略熵 | 稳健、样本效率较高 |

## 8. 控制任务评估

除了 return，还应报告成功率、能耗、动作平滑度、约束违反、恢复能力和控制延迟。高频动作抖动可能获得高奖励却无法部署。

## 自测

1. TD3 的三个改进分别解决什么问题？
2. SAC 的温度参数控制什么？
3. 确定性策略如何探索？
4. 动作缩放不一致会造成什么问题？

[上一节](06-ppo-and-gae.md) · [下一节](08-model-based-imitation-and-offline-rl.md)
