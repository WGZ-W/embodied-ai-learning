# 04 Q-learning 与 DQN

## 1. 从 Q 表到神经网络

DQN 用 $Q_\theta(s,a)$ 近似动作价值。TD 目标：

$$
y=r+\gamma(1-d)\max_{a'}Q_{\bar\theta}(s',a'),
$$

损失：

$$
L(\theta)=E[(Q_\theta(s,a)-y)^2].
$$

$d$ 应表示真正终止，而非任意时间截断。

## 2. 经验回放

Replay Buffer 保存 $(s,a,r,s',d)$ 并随机采样：

- 打破连续样本相关性
- 提高数据复用率
- 使更新成为 off-policy

Buffer 太小导致样本高度相关，太大则可能包含大量过时分布。图像任务常用 frame stack 和压缩存储。

## 3. 目标网络

目标网络参数 $\bar\theta$ 定期硬更新或 Polyak 软更新：

$$
\bar\theta\leftarrow\tau\theta+(1-\tau)\bar\theta.
$$

它减缓目标随在线网络同步变化，降低训练振荡。

## 4. 过估计与 Double DQN

同一个估计器同时选择并评价最大动作会产生正偏差。Double DQN 分离选择与评价：

$$
a^*=\arg\max_aQ_\theta(s',a),
$$

$$
y=r+\gamma Q_{\bar\theta}(s',a^*).
$$

## 5. 常见改进

- Dueling Network：分解状态价值与动作优势
- Prioritized Replay：优先采样大 TD 误差样本，并使用重要性权重修正偏差
- n-step return：加快奖励传播
- Distributional RL：学习回报分布而非仅期望
- Noisy Networks：参数化探索

Rainbow 组合了多项改进，但消融时需逐项验证贡献。

## 6. 训练诊断

应监控 episode return、episode length、TD loss、Q 值尺度、TD error、梯度范数、buffer 大小和探索率。

损失下降不代表策略变好；Q 值持续远超可达到回报范围通常是错误信号。

## 7. 最小实现检查

- 在收集一定数据后再开始学习
- 终止状态不 bootstrap，截断按任务定义处理
- target 张量停止梯度
- 动作索引只取所执行动作的 Q 值
- 定期独立评估并保存最佳/最后检查点
- 固定环境、网络和采样器随机种子

## 自测

1. Replay Buffer 与目标网络分别解决什么问题？
2. Double DQN 如何降低过估计？
3. 为什么 DQN loss 下降不保证 return 上升？
4. 时间截断处理错误会如何影响价值？

[上一节](03-monte-carlo-and-temporal-difference.md) · [下一节](05-policy-gradient-and-actor-critic.md)
