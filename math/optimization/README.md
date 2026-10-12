# 最优化

最优化把学习、估计、规划和控制统一为“在约束下选择最优变量”。本单元重点理解目标函数结构、梯度方法、约束处理和非线性最小二乘。

## 学习目标

- 将实际任务写成决策变量、目标函数和约束
- 判断凸集、凸函数及凸问题
- 掌握梯度下降、Newton 法和拟 Newton 法
- 理解 Lagrange 乘子、KKT 条件与对偶
- 掌握最小二乘、Gauss–Newton 和 Levenberg–Marquardt

## 专题笔记

| 顺序 | 主题 | 笔记 |
| --- | --- | --- |
| 1 | 凸性、一阶方法与收敛诊断 | [01-convexity-and-first-order-methods](notes/01-convexity-and-first-order-methods.md) |
| 2 | 二阶方法与非线性最小二乘 | [02-second-order-and-nonlinear-least-squares](notes/02-second-order-and-nonlinear-least-squares.md) |
| 3 | 约束、对偶与机器人优化 | [03-constrained-and-robotics-optimization](notes/03-constrained-and-robotics-optimization.md) |

## 1. 问题形式

$$
\min_{\mathbf{x}} f(\mathbf{x})
\quad\text{s.t.}\quad
g_i(\mathbf{x})\le0,\ h_j(\mathbf{x})=0.
$$

必须明确决策变量、常量、单位、可行域和停止条件。目标函数权重改变问题含义，不能只靠“调到能跑”。

## 2. 凸优化

凸集内任意两点的连线仍在集合内。可微函数凸当且仅当：

$$
f(\mathbf{y})\ge f(\mathbf{x})+
\nabla f(\mathbf{x})^T(\mathbf{y}-\mathbf{x}).
$$

二阶可微时，$\nabla^2f\succeq0$ 是凸性的判据。凸问题的局部最优即全局最优；非凸问题通常只能寻找局部解或驻点。

## 3. 梯度方法

$$
\mathbf{x}_{k+1}=\mathbf{x}_k-\alpha_k\nabla f(\mathbf{x}_k).
$$

步长可固定、衰减或通过线搜索选择。尺度差异和病态曲率会造成锯齿式收敛，可通过归一化、预条件或二阶方法改善。

Momentum 和 Adam 常用于随机优化；确定性几何问题则常使用线搜索、信赖域和拟 Newton 方法。

## 4. Newton 与拟 Newton

Newton 步：

$$
\Delta\mathbf{x}=-H^{-1}\nabla f,
\qquad
\mathbf{x}_{k+1}=\mathbf{x}_k+\Delta\mathbf{x}.
$$

实际应求解 $H\Delta=-\nabla f$，而非显式求逆。非凸区域的海森矩阵可能不正定，需要阻尼或信赖域。

BFGS/L-BFGS 用梯度差近似曲率，避免显式构造完整海森矩阵。

## 5. 等式约束与 Lagrange 乘子

$$
\mathcal{L}(x,\lambda)=f(x)+\sum_j\lambda_jh_j(x).
$$

驻点满足 $\nabla_x\mathcal{L}=0$ 和 $h_j(x)=0$。几何上，最优点处目标梯度位于约束法向量张成空间中。

## 6. KKT 条件

对于不等式约束，KKT 条件包括：

1. 原始可行性：$g_i(x)\le0$，$h_j(x)=0$
2. 对偶可行性：$\lambda_i\ge0$
3. 互补松弛：$\lambda_i g_i(x)=0$
4. 驻点条件：$\nabla_x\mathcal{L}=0$

在适当正则条件下 KKT 是局部最优必要条件；凸问题中通常也是充分条件。

## 7. 最小二乘

$$
\min_x\frac12\sum_i\lVert r_i(x)\rVert^2.
$$

对残差线性化 $r(x+\Delta)\approx r(x)+J\Delta$，Gauss–Newton 解：

$$
J^TJ\Delta=-J^Tr.
$$

Levenberg–Marquardt 加入阻尼：

$$
(J^TJ+\lambda I)\Delta=-J^Tr.
$$

它在梯度下降的稳健性与 Gauss–Newton 的快速局部收敛之间折中。

## 8. 约束处理

- 投影法：更新后投影回可行集
- 罚函数：把违反约束加入目标
- 障碍函数：靠近边界时代价趋于无穷
- 增广 Lagrangian：结合乘子与罚项

轨迹优化还需利用时间结构和稀疏性，否则大规模问题成本很高。

## 9. 调试检查

- 使用有限差分检查梯度和雅可比
- 检查变量尺度和单位
- 分别报告目标值与约束违反量
- 尝试多个初值判断局部最优敏感性
- 区分求解器“停止”与真正“成功”

## 具身智能应用

逆运动学、相机标定、位姿图优化、轨迹优化、MPC、策略学习和模型训练都可以写成优化问题。

## 完成标准

- [ ] 能识别凸与非凸问题
- [ ] 能实现梯度下降和线搜索
- [ ] 能写出简单问题的 KKT 条件
- [ ] 能推导 Gauss–Newton 更新
- [ ] 能用数值差分检查解析导数
- [ ] 完成[练习与自测](exercises.md)

[返回数学模块](../README.md)
