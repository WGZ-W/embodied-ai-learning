# 机器学习与深度学习

本模块整理从数据中学习预测模型与表征的方法，为强化学习、机器人感知和多模态模型建立统一基础。内容同时关注数学原理、实验方法和可复现实现。

## 学习目标

完成本模块后，应能够：

- 把实际问题表述为监督、无监督或自监督学习任务
- 正确划分数据集，选择损失函数和评估指标
- 理解线性模型、树模型及其适用范围
- 推导神经网络的前向传播和反向传播
- 使用 PyTorch 构建、训练、调试并评估模型
- 解释 CNN、Transformer 和生成模型的核心思想
- 识别过拟合、数据泄漏、分布偏移和训练不稳定问题

## 前置知识

- [线性代数](../math/linear-algebra/README.md)：矩阵运算、最小二乘、特征值与 SVD
- 微积分：偏导数、链式法则和梯度
- 概率统计：随机变量、常见分布、期望与最大似然
- Python 与 NumPy 基础

微积分和概率统计模块尚未整理完成时，可以先掌握笔记中用到的最小概念，再回头补全推导。

## 内容索引

| 顺序 | 主题 | 关键问题 | 笔记 |
| --- | --- | --- | --- |
| 1 | 学习问题与数据 | 模型究竟从什么数据中学习什么？ | [01-learning-problems-and-data](notes/01-learning-problems-and-data.md) |
| 2 | 经典监督学习 | 如何建立可靠的回归与分类基线？ | [02-classical-supervised-learning](notes/02-classical-supervised-learning.md) |
| 3 | 神经网络与反向传播 | 多层网络如何表达并学习复杂函数？ | [03-neural-networks-and-backpropagation](notes/03-neural-networks-and-backpropagation.md) |
| 4 | 优化、泛化与实验 | 如何稳定训练并判断模型是否真正有效？ | [04-optimization-generalization-and-experiments](notes/04-optimization-generalization-and-experiments.md) |
| 5 | 现代架构概览 | 如何根据数据结构选择模型？ | [05-modern-deep-learning-architectures](notes/05-modern-deep-learning-architectures.md) |
| 6 | 无监督与表征学习 | 没有人工标签时如何学习有用表示？ | [06-unsupervised-and-representation-learning](notes/06-unsupervised-and-representation-learning.md) |
| 7 | 卷积神经网络 | 如何利用图像的局部性与空间结构？ | [07-convolutional-neural-networks](notes/07-convolutional-neural-networks.md) |
| 8 | 序列模型与 Transformer | 如何建模时间、上下文和长程依赖？ | [08-sequence-models-and-transformers](notes/08-sequence-models-and-transformers.md) |
| 9 | 生成模型 | 如何学习并采样复杂数据分布？ | [09-generative-models](notes/09-generative-models.md) |
| 10 | 工程、部署与具身应用 | 离线模型如何成为可靠系统组件？ | [10-engineering-deployment-and-embodied-ai](notes/10-engineering-deployment-and-embodied-ai.md) |
| 11 | 练习与自测 | 是否能完成从数据到部署评估的完整闭环？ | [exercises](exercises.md) |

## 建议学习路线

每个主题按照“概念 → 手算 → 最小实现 → 实验对比 → 结论记录”的顺序学习。

| 阶段 | 内容 | 建议产出 |
| --- | --- | --- |
| 第 1 周 | 学习问题、数据、回归与分类 | 完成一个经典监督学习基线 |
| 第 2 周 | 神经网络、反向传播与 PyTorch | 从零实现 MLP，并与 PyTorch 版本对比 |
| 第 3 周 | 优化、正则化与实验设计 | 完成一组可复现的消融实验 |
| 第 4 周 | 无监督学习与表征学习 | 完成 PCA、聚类或对比学习实验 |
| 第 5 周 | CNN 与视觉任务 | 训练一个小型图像分类模型 |
| 第 6 周 | 序列模型与 Transformer | 实现注意力并完成序列建模实验 |
| 第 7 周 | 生成模型 | 理解似然、潜变量与扩散过程 |
| 第 8 周 | 工程、部署与具身应用 | 完成端到端实验报告与部署评估 |

## 核心概念速查

| 概念 | 表达式或判断 | 作用 |
| --- | --- | --- |
| 经验风险 | $\frac{1}{N}\sum_{i=1}^{N}\ell(f_\theta(x_i),y_i)$ | 衡量训练数据上的平均误差 |
| 均方误差 | $\frac{1}{N}\sum_i(\hat y_i-y_i)^2$ | 回归任务常用损失 |
| 交叉熵 | $-\sum_c y_c\log p_c$ | 分类任务常用损失 |
| 梯度下降 | $\theta\leftarrow\theta-\eta\nabla_\theta L$ | 更新模型参数 |
| 正则化 | $L+\lambda\Omega(\theta)$ | 约束模型复杂度 |
| 注意力 | $\operatorname{softmax}(QK^T/\sqrt{d_k})V$ | 动态聚合上下文信息 |

## 实践环境

首轮学习建议使用：

- Python 3.10+
- NumPy、Matplotlib
- scikit-learn
- PyTorch

仓库尚未提供统一环境文件。在加入第一个可运行实验时，再根据真实依赖创建环境配置并固定版本。

## 完成标准

- [ ] 能独立定义训练集、验证集、测试集及评估指标
- [ ] 能训练并解释一个线性或树模型基线
- [ ] 能推导两层 MLP 的梯度传播过程
- [ ] 能使用 PyTorch 完成训练、验证、保存和加载
- [ ] 能通过学习曲线判断欠拟合或过拟合
- [ ] 能说明 CNN 与 Transformer 的主要归纳偏置
- [ ] 能解释 PCA、聚类和对比学习的目标差异
- [ ] 能比较自回归、VAE、GAN 与扩散模型的训练和采样方式
- [ ] 能评估推理延迟、吞吐量、显存和分布外表现
- [ ] 完成基础练习和至少一个综合实验

## 参考资源

- *Dive into Deep Learning*
- *Deep Learning* — Ian Goodfellow, Yoshua Bengio, Aaron Courville
- Stanford CS229: Machine Learning
- Stanford CS231n: Deep Learning for Computer Vision
- PyTorch 官方教程

[返回仓库首页](../README.md)
