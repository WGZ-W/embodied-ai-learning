# 微分方程与动力系统

微分方程描述状态随时间的变化，是机器人动力学、控制、仿真和连续时间世界模型的基础。

## 学习目标

- 理解一阶与高阶常微分方程、初值问题和解的存在性
- 掌握线性系统、平衡点和稳定性分析
- 使用 Euler 与 Runge–Kutta 方法进行数值积分
- 理解刚性、步长、误差和离散化稳定性
- 将机械系统写成状态空间模型

## 专题笔记

| 顺序 | 主题 | 笔记 |
| --- | --- | --- |
| 1 | 常微分方程与状态空间 | [01-odes-and-state-space](notes/01-odes-and-state-space.md) |
| 2 | 平衡点、线性化与稳定性 | [02-linearization-and-stability](notes/02-linearization-and-stability.md) |
| 3 | 数值积分与机械系统仿真 | [03-numerical-integration-and-mechanics](notes/03-numerical-integration-and-mechanics.md) |

## 1. 初值问题

$$
\dot{\mathbf{x}}=f(t,\mathbf{x}),
\qquad \mathbf{x}(t_0)=\mathbf{x}_0.
$$

状态 $\mathbf{x}$ 应包含预测未来所需的最小信息。二阶方程可通过引入速度改写为一阶系统。

## 2. 一阶方程

可分离方程通过变量分离积分。线性一阶方程：

$$
\dot x+a(t)x=b(t)
$$

可使用积分因子求解。解析解有助于理解，但复杂机器人系统通常依赖数值方法。

## 3. 线性时不变系统

$$
\dot{\mathbf{x}}=A\mathbf{x}+B\mathbf{u},
\qquad
\mathbf{y}=C\mathbf{x}+D\mathbf{u}.
$$

无输入解为：

$$
\mathbf{x}(t)=e^{At}\mathbf{x}(0).
$$

连续系统渐近稳定要求 $A$ 的全部特征值实部为负。离散系统 $x_{k+1}=Ax_k$ 稳定则要求谱半径小于 1。

## 4. 非线性系统与线性化

平衡点满足 $f(x^*,u^*)=0$。在其附近：

$$
\delta\dot x\approx A\delta x+B\delta u,
$$

$$
A=\left.\frac{\partial f}{\partial x}\right|_{x^*,u^*},
\qquad
B=\left.\frac{\partial f}{\partial u}\right|_{x^*,u^*}.
$$

线性化只描述局部行为，远离工作点时结论可能失效。

## 5. 相图与稳定性

相图显示状态轨迹和向量场。稳定性描述小扰动后的行为：

- Lyapunov 稳定：轨迹保持靠近
- 渐近稳定：轨迹最终回到平衡点
- 指数稳定：以指数速度收敛

Lyapunov 方法寻找正定函数 $V(x)$，若 $\dot V(x)<0$，可证明平衡点稳定而无需显式求解 ODE。

## 6. 数值积分

显式 Euler：

$$
x_{k+1}=x_k+h f(t_k,x_k).
$$

经典 RK4：

$$
k_1=f(t_k,x_k),
$$

$$
k_2=f(t_k+h/2,x_k+hk_1/2),
$$

$$
k_3=f(t_k+h/2,x_k+hk_2/2),
$$

$$
k_4=f(t_k+h,x_k+hk_3),
$$

$$
x_{k+1}=x_k+\frac h6(k_1+2k_2+2k_3+k_4).
$$

减小步长通常提高精度但增加计算量。刚性系统可能迫使显式方法使用极小步长，此时应考虑隐式方法。

## 7. 机械系统

机械臂动力学常写为：

$$
M(q)\ddot q+C(q,\dot q)\dot q+g(q)=\tau.
$$

令状态 $x=[q^T,\dot q^T]^T$，即可转换为一阶状态方程。仿真器实际求解的是离散时间近似，步长、接触模型和求解器会影响结果。

## 8. 数值实验规范

- 与具有解析解的问题比较误差
- 缩小步长检查收敛阶
- 监控能量、约束和状态边界
- 区分模型误差与积分误差
- 记录求解器、容差和步长

## 完成标准

- [ ] 能把二阶系统转换为一阶状态空间
- [ ] 能用特征值判断线性系统稳定性
- [ ] 能对非线性系统做局部线性化
- [ ] 能实现 Euler 和 RK4 并比较误差
- [ ] 能解释刚性与数值不稳定
- [ ] 完成[练习与自测](exercises.md)

[返回数学模块](../README.md)
