# 03 神经网络与反向传播

## 1. 从线性模型到多层感知机

线性层执行仿射变换：

$$
\mathbf{z}=W\mathbf{x}+\mathbf{b}.
$$

多个线性层直接组合仍然等价于一个线性层。神经网络在层之间加入非线性激活函数，从而表达复杂函数：

$$
\mathbf{h}=\phi(W_1\mathbf{x}+\mathbf{b}_1),
\qquad
\hat{\mathbf{y}}=W_2\mathbf{h}+\mathbf{b}_2.
$$

常见激活函数包括 ReLU、GELU、Sigmoid 和 Tanh。隐藏层通常使用 ReLU 或 GELU；Sigmoid 常用于二分类输出，Tanh 常用于需要有界输出的场景。

## 2. 张量与批处理

深度学习中的数据通常以张量表示。需要始终明确每一维的语义：

- 表格数据：`[batch, features]`
- 图像数据：`[batch, channels, height, width]`
- 序列数据：`[batch, sequence, features]`

批处理把多个样本的计算合并，提高硬件利用率，并使用小批量梯度近似完整数据集梯度。

## 3. 计算图

前向传播可以表示为一张有向无环计算图。每个节点保存中间结果，每条边表示依赖关系。

例如：

$$
z=wx+b,\qquad \hat y=\sigma(z),\qquad L=\ell(\hat y,y).
$$

反向传播沿计算图反方向，使用链式法则计算每个参数对最终损失的影响。

## 4. 链式法则与反向传播

若 $L$ 依赖 $y$，而 $y$ 依赖 $x$，则：

$$
\frac{\partial L}{\partial x}
=\frac{\partial L}{\partial y}
\frac{\partial y}{\partial x}.
$$

以标量两层网络为例：

$$
z_1=w_1x+b_1,\quad h=\phi(z_1),
\quad \hat y=w_2h+b_2.
$$

则：

$$
\frac{\partial L}{\partial w_2}
=\frac{\partial L}{\partial \hat y}h,
$$

$$
\frac{\partial L}{\partial w_1}
=\frac{\partial L}{\partial \hat y}
w_2\phi'(z_1)x.
$$

反向传播不是一种优化器，而是一种高效计算梯度的方法。优化器使用这些梯度更新参数。

## 5. 自动微分

PyTorch 在前向计算时构建动态计算图，调用 `loss.backward()` 后自动计算梯度：

```python
import torch
from torch import nn

model = nn.Sequential(
    nn.Linear(4, 16),
    nn.ReLU(),
    nn.Linear(16, 2),
)

x = torch.randn(8, 4)
y = torch.randint(0, 2, (8,))

logits = model(x)
loss = nn.CrossEntropyLoss()(logits, y)
loss.backward()

for name, parameter in model.named_parameters():
    print(name, parameter.grad.shape)
```

默认情况下梯度会累积，因此每次更新前需要清零：

```python
optimizer.zero_grad()
loss.backward()
optimizer.step()
```

## 6. 参数初始化

初始化过大可能导致激活或梯度爆炸，过小可能导致信号逐层衰减。常见方法包括：

- Xavier/Glorot 初始化：适合 Tanh 等激活。
- Kaiming/He 初始化：适合 ReLU 系列激活。
- 偏置通常初始化为零或小常数。

现代框架通常提供合理默认值，但理解初始化有助于排查训练失败。

## 7. 梯度问题

### 梯度消失

深层网络中梯度逐层变小，早期层难以学习。常见缓解方法：ReLU、残差连接、归一化和合适初始化。

### 梯度爆炸

梯度快速增大，导致损失出现 `NaN` 或训练发散。常见处理：降低学习率、梯度裁剪、归一化和检查异常输入。

## 8. 梯度检查

手写算子或复杂损失时，可以用有限差分验证梯度：

$$
\frac{\partial L}{\partial\theta}
\approx\frac{L(\theta+\epsilon)-L(\theta-\epsilon)}{2\epsilon}.
$$

梯度检查只适合小模型和调试，不用于正常训练。

## 9. 最小训练循环

```python
for x, y in train_loader:
    optimizer.zero_grad()
    prediction = model(x)
    loss = criterion(prediction, y)
    loss.backward()
    optimizer.step()
```

训练循环之外还需要验证模式、指标汇总、检查点保存、随机种子和日志记录，才能形成可靠实验。

## 自测

1. 为什么多个线性层之间必须加入非线性？
2. 反向传播与梯度下降有什么区别？
3. 为什么 PyTorch 更新前需要清空梯度？
4. 如何初步判断训练失败来自梯度爆炸？

[上一节](02-classical-supervised-learning.md) · [下一节：优化、泛化与实验](04-optimization-generalization-and-experiments.md)
