# 01 向量、矩阵与线性映射

## 1. 向量是什么

向量可以从三个互补角度理解：

- **数据视角**：一组有顺序的数，例如机器人状态 $[x,y,\theta]^T$。
- **几何视角**：空间中的方向和大小。
- **代数视角**：向量空间中的元素，可以进行加法和数乘。

列向量通常写作：

$$
\mathbf{x}=\begin{bmatrix}x_1\\x_2\\\vdots\\x_n\end{bmatrix}\in\mathbb{R}^n.
$$

向量的二范数表示长度：

$$
\lVert\mathbf{x}\rVert_2=\sqrt{\mathbf{x}^T\mathbf{x}}.
$$

非零向量的单位向量为 $\hat{\mathbf{x}}=\mathbf{x}/\lVert\mathbf{x}\rVert_2$。

## 2. 线性组合、张成与基

向量 $\mathbf{v}_1,\ldots,\mathbf{v}_k$ 的线性组合是：

$$
\alpha_1\mathbf{v}_1+\cdots+\alpha_k\mathbf{v}_k.
$$

- 所有线性组合构成这些向量的**张成空间**。
- 如果只有所有系数均为零时组合结果才是零向量，则这些向量**线性无关**。
- 能够张成空间且线性无关的一组向量称为一组**基**。
- 基中向量的数量就是空间的**维数**。

基不是唯一的。同一个几何向量在不同基下具有不同坐标，这正是坐标变换需要处理的问题。

## 3. 矩阵是什么

矩阵 $A\in\mathbb{R}^{m\times n}$ 可以理解为从 $\mathbb{R}^n$ 到 $\mathbb{R}^m$ 的线性映射：

$$
\mathbf{y}=A\mathbf{x}.
$$

矩阵的第 $j$ 列是第 $j$ 个标准基向量经过该映射后的结果。因此，$A\mathbf{x}$ 也可以理解为用 $\mathbf{x}$ 的分量对矩阵各列进行线性组合。

线性映射满足：

$$
A(\alpha\mathbf{x}+\beta\mathbf{y})
=\alpha A\mathbf{x}+\beta A\mathbf{y}.
$$

## 4. 矩阵乘法与复合变换

若 $A\in\mathbb{R}^{m\times n}$、$B\in\mathbb{R}^{n\times p}$，则：

$$
C=AB\in\mathbb{R}^{m\times p}.
$$

$AB\mathbf{x}$ 表示先应用 $B$，再应用 $A$。矩阵乘法通常不满足交换律，即 $AB\neq BA$。

计算前应先检查形状：

```python
import numpy as np

A = np.array([[1.0, 2.0], [0.0, 1.0]])  # (2, 2)
x = np.array([3.0, 4.0])                  # (2,)
y = A @ x                                 # (2,)
print(y)
```

## 5. 转置、逆与常见矩阵

- 转置：$(A^T)_{ij}=A_{ji}$。
- 单位矩阵：$I\mathbf{x}=\mathbf{x}$。
- 对称矩阵：$A=A^T$。
- 对角矩阵：只有主对角线元素可能非零。
- 正交矩阵：$Q^TQ=I$，因此 $Q^{-1}=Q^T$。
- 可逆矩阵：存在 $A^{-1}$ 使 $A^{-1}A=I$。

不要在数值计算中显式求逆来解 $A\mathbf{x}=\mathbf{b}$。优先使用线性方程求解器：

```python
x = np.linalg.solve(A, b)  # 优于 np.linalg.inv(A) @ b
```

## 6. 内积、夹角与正交

欧氏内积为：

$$
\langle\mathbf{x},\mathbf{y}\rangle=\mathbf{x}^T\mathbf{y}.
$$

夹角满足：

$$
\cos\theta=\frac{\mathbf{x}^T\mathbf{y}}
{\lVert\mathbf{x}\rVert_2\lVert\mathbf{y}\rVert_2}.
$$

若 $\mathbf{x}^T\mathbf{y}=0$，则两个向量正交。正交性在坐标轴构造、误差分解和投影中非常重要。

## 7. 具身智能中的例子

- 机器人关节状态可以表示为向量 $\mathbf{q}$。
- 雅可比矩阵把关节速度映射为末端速度：$\dot{\mathbf{x}}=J(\mathbf{q})\dot{\mathbf{q}}$。
- 图像可以表示为矩阵，点云可以表示为 $N\times3$ 的矩阵。
- 神经网络线性层执行 $\mathbf{y}=W\mathbf{x}+\mathbf{b}$。

## 自测

1. 为什么 $A\mathbf{x}$ 可以看作 $A$ 的列向量的线性组合？
2. $A\in\mathbb{R}^{3\times4}$、$B\in\mathbb{R}^{4\times2}$ 时，$AB$ 的形状是什么？
3. 正交矩阵为什么不会改变向量长度？

[上一层](../README.md) · [下一节：线性方程组与最小二乘](02-linear-systems-and-least-squares.md)
