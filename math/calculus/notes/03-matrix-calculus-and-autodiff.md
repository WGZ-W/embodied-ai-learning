# 03 矩阵微分、计算图与自动微分

## 1. 先确定布局约定

矩阵微分存在 numerator-layout 与 denominator-layout 等约定。本文采用：标量对列向量求导得到列梯度，向量对向量求导得到“输出维 × 输入维”的雅可比。

推导前先写形状，能避免大多数转置错误。

## 2. 微分法

比逐元素偏导更稳健的方法是先写全微分，再整理为：

$$
df=(\nabla_x f)^Tdx.
$$

例如：

$$
f=x^TAx,
$$

$$
df=(dx)^TAx+x^TAdx
=x^T(A^T+A)dx,
$$

因此：

$$
\nabla_xf=(A+A^T)x.
$$

## 3. 常用结果

$$
\nabla_x(a^Tx)=a,
\qquad
\nabla_x\frac12\lVert Ax-b\rVert^2=A^T(Ax-b).
$$

$$
\frac{\partial}{\partial X}\operatorname{tr}(A^TX)=A,
$$

$$
d(X^{-1})=-X^{-1}(dX)X^{-1},
$$

$$
d\log\det X=\operatorname{tr}(X^{-1}dX).
$$

逆矩阵和 log-determinant 公式常用于高斯似然和优化。

## 4. 向量—Jacobian 积

反向模式通常不显式构造完整雅可比，而计算：

$$
\mathbf{v}^TJ.
$$

从标量损失开始，$v=1$，每个算子把上游伴随量乘以局部雅可比传回输入。这正是反向传播节省内存和计算的关键。

前向模式计算 $Jv$，适合输入维数小；反向模式计算 $v^TJ$，适合输出为标量、参数很多的模型。

## 5. 计算图中的梯度累积

若一个变量通过多条路径影响损失，其梯度等于各路径贡献之和。因此框架默认累积参数梯度。

原地修改中间变量、错误地 `detach`、把张量转成普通数值，都可能切断计算图。

## 6. 梯度检查

方向导数检查比逐坐标检查更便宜：

$$
\nabla f(x)^Tv
\approx\frac{f(x+\epsilon v)-f(x-\epsilon v)}{2\epsilon}.
$$

应使用双精度、小模型、平滑点，并尝试多个 $\epsilon$。ReLU 折点等不可导位置不适合有限差分检查。

## 7. 高阶导数

Newton 法、隐式层、元学习和物理约束可能需要 Hessian-vector product：

$$
Hv=\left.\frac{d}{d\epsilon}\nabla f(x+\epsilon v)\right|_{\epsilon=0}.
$$

它无需显式存储 $n\times n$ 海森矩阵，适合大规模问题。

## 8. 数值注意事项

- 使用 `logsumexp` 避免指数溢出。
- 解线性方程而不是显式求逆。
- 检查梯度范数、NaN 和 Inf。
- 广播可能让代码运行但导数语义错误。
- 对角度、四元数和流形变量，欧氏加法未必保持约束。

## 自测

1. 如何用微分法推导二次型梯度？
2. 反向模式为何适合神经网络训练？
3. 一个变量分叉到两条路径时梯度如何组合？
4. 为什么不应在不可导点做中心差分检查？

[上一节](02-multivariable-calculus.md) · [练习](../exercises.md)
