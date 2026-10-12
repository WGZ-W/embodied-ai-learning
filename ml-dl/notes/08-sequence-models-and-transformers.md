# 08 序列模型与 Transformer

## 1. 序列建模问题

序列数据同时包含内容和顺序。常见任务包括：

- 序列到标签：用一段传感器序列判断故障。
- 序列到序列：把观测历史映射为动作序列。
- 自回归预测：根据过去预测下一状态或 token。
- 序列编码：把可变长度输入映射为固定维表示。

训练前必须定义时间窗口、预测范围、因果约束和缺失数据处理。随机打乱时间点通常会泄漏未来信息。

## 2. RNN

基本 RNN 更新：

$$
h_t=\phi(W_xx_t+W_hh_{t-1}+b),
\qquad y_t=W_yh_t+b_y.
$$

参数在时间上共享。训练使用时间反向传播（BPTT），梯度会连续乘以循环雅可比，因此容易消失或爆炸。

LSTM 和 GRU 使用门控控制信息写入、保留和输出，能缓解长期依赖问题，但仍难以并行处理长序列。

## 3. Padding 与 Mask

批处理中不同长度序列通常补齐到相同长度。Padding 位置不应影响损失或注意力，因此需要 mask。

两类 mask 不应混淆：

- Padding mask：屏蔽补齐位置。
- Causal mask：屏蔽未来位置，保证自回归模型不能偷看答案。

错误 mask 可能不报错，却会让验证结果虚高或训练彻底失败。

## 4. 自注意力推导

输入 $X\in\mathbb{R}^{n\times d}$ 经过线性映射：

$$
Q=XW_Q,\quad K=XW_K,\quad V=XW_V.
$$

相关性得分与输出为：

$$
S=\frac{QK^T}{\sqrt{d_k}},
\qquad A=\operatorname{softmax}(S),
\qquad O=AV.
$$

$A_{ij}$ 表示第 $i$ 个位置从第 $j$ 个位置聚合多少信息。Softmax 沿 key 维计算，每行权重之和为 1。

## 5. 多头注意力

每个头使用独立投影：

$$
\operatorname{head}_i=\operatorname{Attention}(Q_i,K_i,V_i).
$$

各头拼接后再进行输出投影。多头结构允许模型同时关注局部、长程、语义或跨模态等不同关系，但头数增加不必然提升性能。

## 6. Transformer 块

典型 Pre-Norm 块：

$$
X'=X+\operatorname{MHA}(\operatorname{LN}(X)),
$$

$$
Y=X'+\operatorname{MLP}(\operatorname{LN}(X')).
$$

关键组件包括：

- 多头注意力：跨位置交换信息。
- 前馈网络：对每个位置独立进行非线性变换。
- 残差连接：改善梯度传播。
- LayerNorm：稳定特征尺度。
- 位置表示：注入顺序信息。

## 7. Encoder、Decoder 与 Cross-Attention

- Encoder 使用双向自注意力，适合理解完整输入。
- Decoder 使用因果自注意力，适合逐步生成。
- Encoder–Decoder 模型用 Cross-Attention 让输出序列查询输入表示。

在视觉语言控制中，语言 token、视觉 token 和动作 token 可以通过自注意力或交叉注意力交互。

## 8. 位置表示

没有位置表示时，自注意力对 token 排列近似置换等变。常见方法：

- 固定正弦位置编码。
- 可学习绝对位置嵌入。
- 相对位置偏置。
- RoPE 等旋转位置表示。

选择时需关注训练长度外推、二维空间结构和缓存推理需求。

## 9. 训练与推理

自回归训练通常使用 Teacher Forcing：一次并行预测所有下一 token。推理时只能逐步生成，形成训练—推理暴露差异。

常见解码策略：

- Greedy：每步选择最高概率。
- Beam Search：保留多个高分候选。
- Temperature、Top-k、Top-p：控制随机采样范围。

动作生成需要额外考虑连续性、物理可行性和实时延迟，不能直接照搬文本采样策略。

## 10. 长序列与缓存

标准注意力对长度 $n$ 的复杂度为 $O(n^2)$。常见改进包括局部窗口、稀疏注意力、分块处理和状态压缩。

自回归推理可缓存过去的 Key/Value，避免重复计算，但缓存显存随序列增长。机器人在线系统需要限制历史长度或压缩记忆。

## 11. 具身智能中的应用

- 使用观测历史处理部分可观测性。
- 将视觉、语言、本体感知和动作统一为 token 序列。
- 预测动作 chunk，减少逐步推理开销。
- 学习轨迹模型、世界模型或视觉—语言—动作策略。

评估除离线损失外，还应检查闭环成功率、动作延迟、扰动恢复和长时间误差累积。

## 自测

1. Padding mask 与 Causal mask 分别解决什么问题？
2. 注意力为什么除以 $\sqrt{d_k}$？
3. Teacher Forcing 会造成什么训练—推理差异？
4. KV Cache 用计算换取了什么资源？

[上一节](07-convolutional-neural-networks.md) · [下一节：生成模型](09-generative-models.md)
