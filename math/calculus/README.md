# 微积分

微积分研究连续变化，是反向传播、机器人运动学、动力学和优化的共同语言。本单元重点掌握“变化率、局部线性化、累积量”三个核心思想。

## 学习目标

- 理解极限、连续、导数和积分的几何含义
- 熟练使用乘积、商和链式法则
- 计算多元函数的梯度、雅可比与海森矩阵
- 使用 Taylor 展开进行局部近似
- 理解矩阵微分和自动微分的基本机制

## 专题笔记

| 顺序 | 主题 | 笔记 |
| --- | --- | --- |
| 1 | 极限、单变量微分与积分 | [01-single-variable-calculus](notes/01-single-variable-calculus.md) |
| 2 | 多元微分、局部近似与误差传播 | [02-multivariable-calculus](notes/02-multivariable-calculus.md) |
| 3 | 矩阵微分、计算图与自动微分 | [03-matrix-calculus-and-autodiff](notes/03-matrix-calculus-and-autodiff.md) |

## 1. 极限与连续

极限描述输入趋近某点时函数值的行为：

$$
\lim_{x\to a}f(x)=L.
$$

函数在 $a$ 连续要求极限存在且等于 $f(a)$。连续不保证可导；可导意味着函数在该点附近能够被线性函数良好近似。

## 2. 单变量微分

导数定义：

$$
f'(x)=\lim_{h\to0}\frac{f(x+h)-f(x)}{h}.
$$

常用规则：

$$
(fg)'=f'g+fg',\qquad
\left(\frac{f}{g}\right)'=\frac{f'g-fg'}{g^2},
$$

$$
\frac{d}{dx}f(g(x))=f'(g(x))g'(x).
$$

链式法则是反向传播的核心：复杂函数可以拆为局部导数的乘积。

## 3. 多元微分

标量函数 $f:\mathbb{R}^n\to\mathbb{R}$ 的梯度：

$$
\nabla f=\begin{bmatrix}
\partial f/\partial x_1&\cdots&\partial f/\partial x_n
\end{bmatrix}^T.
$$

梯度指向函数增长最快的方向，方向导数为：

$$
D_{\mathbf{u}}f=\nabla f^T\mathbf{u}.
$$

向量函数 $\mathbf{f}:\mathbb{R}^n\to\mathbb{R}^m$ 的雅可比：

$$
J_{ij}=\frac{\partial f_i}{\partial x_j}.
$$

机器人雅可比将关节速度映射到末端速度：

$$
\dot{\mathbf{x}}=J(\mathbf{q})\dot{\mathbf{q}}.
$$

## 4. 海森矩阵与二阶信息

标量函数的海森矩阵由二阶偏导组成：

$$
H_{ij}=\frac{\partial^2f}{\partial x_i\partial x_j}.
$$

在驻点 $\nabla f=0$ 附近：

- $H\succ0$：局部极小值
- $H\prec0$：局部极大值
- $H$ 不定：鞍点

## 5. Taylor 展开

单变量二阶近似：

$$
f(x+h)\approx f(x)+f'(x)h+\frac12f''(x)h^2.
$$

多变量二阶近似：

$$
f(\mathbf{x}+\Delta)\approx f(\mathbf{x})
+\nabla f^T\Delta+\frac12\Delta^TH\Delta.
$$

局部线性化用于 EKF、控制和误差传播；二阶近似用于 Newton 法和局部稳定性分析。

## 6. 积分

定积分表示连续累积：

$$
\int_a^bf(x)\,dx.
$$

微积分基本定理连接导数与积分：

$$
\frac{d}{dx}\int_a^xf(t)\,dt=f(x).
$$

在机器人学中，速度积分得到位移，概率密度积分得到概率，连续代价积分得到轨迹总代价。

## 7. 矩阵微分速查

$$
\nabla_{\mathbf{x}}(\mathbf{a}^T\mathbf{x})=\mathbf{a},
$$

$$
\nabla_{\mathbf{x}}(\mathbf{x}^TA\mathbf{x})=(A+A^T)\mathbf{x},
$$

若 $A$ 对称，则结果为 $2A\mathbf{x}$。

$$
\nabla_{\mathbf{x}}\lVert A\mathbf{x}-\mathbf{b}\rVert_2^2
=2A^T(A\mathbf{x}-\mathbf{b}).
$$

## 8. 自动微分

- Forward mode 沿计算方向传播导数，适合输入少、输出多。
- Reverse mode 从输出反向传播伴随量，适合参数多、标量损失；反向传播属于此类。
- 自动微分不同于数值差分：它按计算图精确组合局部导数，不依赖有限差分步长。

## 学习顺序

极限与导数 → 链式法则 → 偏导与梯度 → 雅可比与海森矩阵 → Taylor 展开 → 积分 → 自动微分。

## 完成标准

- [ ] 能独立计算复合函数和多元函数导数
- [ ] 能根据输入输出维度写出雅可比形状
- [ ] 能使用 Taylor 展开线性化非线性函数
- [ ] 能推导最小二乘目标的梯度
- [ ] 能解释反向传播与数值差分的区别
- [ ] 完成[练习与自测](exercises.md)

## 参考资源

- MIT 18.01 Single Variable Calculus
- MIT 18.02 Multivariable Calculus
- *The Matrix Cookbook*
- 3Blue1Brown, *Essence of Calculus*

[返回数学模块](../README.md)
