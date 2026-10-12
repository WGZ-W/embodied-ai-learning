# 06 PPO 与 GAE

## 1. 为什么限制策略更新

策略变化过大会使旧数据失效，也可能让性能突然崩溃。TRPO 使用 KL 约束，PPO 用更易实现的裁剪代理目标近似限制更新。

## 2. 概率比

$$
r_t(\theta)=\frac{\pi_\theta(a_t\mid s_t)}
{\pi_{old}(a_t\mid s_t)}
=\exp(\log\pi_\theta-\log\pi_{old}).
$$

旧 log-prob 必须在采样时保存并停止梯度。

## 3. PPO-Clip

$$
L^{clip}=E\left[
\min(r_t\hat A_t,
\operatorname{clip}(r_t,1-\epsilon,1+\epsilon)\hat A_t)
\right].
$$

`min` 针对正负优势选择保守改进。裁剪不是严格 KL 约束，仍应监控 approximate KL 和 clip fraction。

## 4. GAE

$$
\delta_t=r_t+\gamma V(s_{t+1})-V(s_t),
$$

$$
\hat A_t^{GAE}=\sum_{l=0}^{T-t-1}(\gamma\lambda)^l\delta_{t+l}.
$$

$\lambda$ 小：偏差高、方差低；$\lambda$ 大：更接近 Monte Carlo，偏差低、方差高。

## 5. Value target

$$
\hat V_t=\hat A_t+V_{old}(s_t).
$$

Value loss、policy loss 与 entropy bonus 共同优化。三者尺度不同，需要监控而非只看总 loss。

## 6. 批次与 epoch

PPO 收集一批当前策略数据，再随机打乱成 mini-batch 做多个 epoch。epoch 太少数据利用不足，太多会让策略偏离采样策略。

## 7. 实现检查

- 按时间反向计算 GAE
- 在 episode boundary 重置递推；截断可按最终状态 bootstrap
- 对优势做标准化
- 记录 old log-prob 和 old value
- 对连续动作正确汇总各维 log-prob
- 监控 entropy、KL、clip fraction、explained variance

## 8. Explained variance

$$
1-\frac{\operatorname{Var}(y-\hat y)}{\operatorname{Var}(y)}.
$$

接近 1 表示 Critic 能解释回报变化；接近 0 表示不优于常数；负值表示非常差。但它不能单独决定策略质量。

## 自测

1. PPO 裁剪为何要结合优势符号理解？
2. GAE 的 $\lambda$ 控制什么权衡？
3. 同一批数据训练太多 epoch 有什么风险？
4. 为什么总 loss 不适合作为唯一诊断？

[上一节](05-policy-gradient-and-actor-critic.md) · [下一节](07-continuous-control.md)
