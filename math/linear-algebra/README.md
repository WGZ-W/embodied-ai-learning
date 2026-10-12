# 线性代数

线性代数是具身智能中表示空间、描述变换和求解估计问题的基础工具。本单元不追求覆盖所有理论，而是围绕机器人学、计算机视觉、状态估计和机器学习中的高频概念建立可使用的知识框架。

## 学习目标

完成本单元后，应能够：

- 使用向量和矩阵表示状态、数据及线性映射
- 判断线性方程组是否有解，并理解秩、零空间和条件数
- 使用投影与最小二乘处理超定方程组
- 解释特征值分解和奇异值分解的几何意义
- 使用旋转矩阵和齐次变换描述刚体位姿
- 使用 NumPy 验证推导并检查数值结果

## 前置知识

- 基本代数运算
- 平面与空间直角坐标系
- Python 基础；实践部分需要 NumPy

## 内容索引

| 顺序 | 主题 | 关键问题 | 笔记 |
| --- | --- | --- | --- |
| 1 | 向量、矩阵与线性映射 | 数据和空间变换如何表示？ | [01-vectors-and-matrices](notes/01-vectors-and-matrices.md) |
| 2 | 线性方程组、秩与最小二乘 | 无解或多解时如何判断与估计？ | [02-linear-systems-and-least-squares](notes/02-linear-systems-and-least-squares.md) |
| 3 | 特征值与矩阵分解 | 如何找到主要方向并压缩信息？ | [03-eigenvalues-and-decompositions](notes/03-eigenvalues-and-decompositions.md) |
| 4 | 旋转与刚体变换 | 如何统一描述不同坐标系下的位姿？ | [04-rotations-and-transformations](notes/04-rotations-and-transformations.md) |
| 5 | 练习与自测 | 是否能独立计算、解释并实现？ | [exercises](exercises.md) |

## 建议学习顺序

每一节按照以下闭环完成：

1. 阅读概念和公式，明确每个量的形状。
2. 手算一个二维或三维例子。
3. 使用 NumPy 验证结果。
4. 用自己的话解释几何意义。
5. 完成对应练习后再进入下一节。

建议用两周完成首轮学习：

| 时间 | 内容 | 产出 |
| --- | --- | --- |
| 第 1–2 天 | 向量、矩阵与线性映射 | 能检查形状并解释矩阵乘法 |
| 第 3–5 天 | 线性方程组、秩、投影与最小二乘 | 能求解并判断解的可靠性 |
| 第 6–8 天 | 特征值、EVD 与 SVD | 能解释主方向和低秩近似 |
| 第 9–11 天 | 旋转矩阵与齐次变换 | 能完成坐标系之间的点变换 |
| 第 12–14 天 | 综合练习与复习 | 完成自测和一个 NumPy 小实验 |

## 核心公式速查

| 概念 | 公式 | 用途 |
| --- | --- | --- |
| 线性方程组 | $A\mathbf{x}=\mathbf{b}$ | 标定、估计、约束求解 |
| 内积 | $\mathbf{x}^{T}\mathbf{y}$ | 相似度、夹角、投影 |
| 最小二乘 | $\hat{\mathbf{x}}=\arg\min_{\mathbf{x}}\lVert A\mathbf{x}-\mathbf{b}\rVert_2^2$ | 含噪数据拟合 |
| 特征值 | $A\mathbf{v}=\lambda\mathbf{v}$ | 稳定性、主方向分析 |
| SVD | $A=U\Sigma V^T$ | 低秩近似、伪逆、PCA |
| 三维旋转 | $\mathbf{p}'=R\mathbf{p}$ | 坐标轴或刚体姿态变换 |
| 刚体变换 | $\tilde{\mathbf{p}}'=T\tilde{\mathbf{p}}$ | 同时表示旋转和平移 |

## 完成标准

- [ ] 能解释线性组合、张成、线性无关、基和维数
- [ ] 能通过秩判断方程组的解结构
- [ ] 能推导普通最小二乘的正规方程
- [ ] 能说明特征值分解与 SVD 的区别
- [ ] 能验证旋转矩阵满足 $R^TR=I$ 且 $\det(R)=1$
- [ ] 能组合并求逆两个齐次变换
- [ ] 完成练习中的基础题和至少一道综合题

## 参考资源

- Gilbert Strang, *Introduction to Linear Algebra*
- Deisenroth, Faisal, Ong, *Mathematics for Machine Learning*
- MIT 18.06 Linear Algebra
- 3Blue1Brown, *Essence of Linear Algebra*

[返回数学模块](../README.md) · [返回仓库首页](../../README.md)
