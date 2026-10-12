# 02 线性方程组、秩与最小二乘

## 1. 线性方程组

线性方程组统一写作：

$$
A\mathbf{x}=\mathbf{b},
$$

其中 $A\in\mathbb{R}^{m\times n}$，$\mathbf{x}\in\mathbb{R}^n$ 是未知量，$\mathbf{b}\in\mathbb{R}^m$ 是观测或约束。

从几何上看，求解问题是在寻找一个 $\mathbf{x}$，使矩阵列向量的线性组合等于 $\mathbf{b}$。因此，方程有解当且仅当 $\mathbf{b}$ 位于 $A$ 的列空间中。

## 2. 四个基本子空间

对 $A\in\mathbb{R}^{m\times n}$：

- 列空间 $\mathcal{C}(A)\subseteq\mathbb{R}^m$：所有可能的 $A\mathbf{x}$。
- 零空间 $\mathcal{N}(A)\subseteq\mathbb{R}^n$：满足 $A\mathbf{x}=0$ 的向量。
- 行空间 $\mathcal{C}(A^T)\subseteq\mathbb{R}^n$。
- 左零空间 $\mathcal{N}(A^T)\subseteq\mathbb{R}^m$。

列空间维数与行空间维数相等，称为矩阵的秩：

$$
\operatorname{rank}(A)=r.
$$

秩—零度定理给出：

$$
\operatorname{rank}(A)+\operatorname{nullity}(A)=n.
$$

## 3. 解的结构

- **唯一解**：$A$ 的列线性无关，且 $\mathbf{b}\in\mathcal{C}(A)$。
- **无穷多解**：方程相容，但 $\mathcal{N}(A)$ 中存在非零向量。
- **无解**：$\mathbf{b}\notin\mathcal{C}(A)$。

若 $\mathbf{x}_p$ 是一个特解，则所有解为：

$$
\mathbf{x}=\mathbf{x}_p+\mathbf{x}_n,
\qquad \mathbf{x}_n\in\mathcal{N}(A).
$$

## 4. 投影

将向量 $\mathbf{b}$ 投影到非零向量 $\mathbf{a}$ 所在直线上：

$$
\operatorname{proj}_{\mathbf{a}}(\mathbf{b})
=\mathbf{a}\frac{\mathbf{a}^T\mathbf{b}}{\mathbf{a}^T\mathbf{a}}.
$$

投影矩阵为：

$$
P=\frac{\mathbf{a}\mathbf{a}^T}{\mathbf{a}^T\mathbf{a}}.
$$

若 $A$ 列满秩，则投影到 $A$ 的列空间的矩阵为：

$$
P=A(A^TA)^{-1}A^T.
$$

投影残差 $\mathbf{e}=\mathbf{b}-P\mathbf{b}$ 与列空间正交，因此 $A^T\mathbf{e}=0$。

## 5. 最小二乘

当观测含噪或约束过多时，$A\mathbf{x}=\mathbf{b}$ 往往没有精确解。最小二乘寻找残差最小的解：

$$
\hat{\mathbf{x}}
=\arg\min_{\mathbf{x}}\lVert A\mathbf{x}-\mathbf{b}\rVert_2^2.
$$

令目标函数对 $\mathbf{x}$ 的梯度为零，可得正规方程：

$$
A^TA\hat{\mathbf{x}}=A^T\mathbf{b}.
$$

当 $A$ 列满秩时：

$$
\hat{\mathbf{x}}=(A^TA)^{-1}A^T\mathbf{b}=A^+\mathbf{b},
$$

其中 $A^+$ 是 Moore–Penrose 伪逆。

实际计算时不要显式构造 $(A^TA)^{-1}$。`np.linalg.lstsq` 通常基于更稳定的分解方法：

```python
import numpy as np

A = np.array([[1.0, 0.0], [1.0, 1.0], [1.0, 2.0]])
b = np.array([1.0, 2.0, 2.8])

x_hat, residuals, rank, singular_values = np.linalg.lstsq(A, b, rcond=None)
print(x_hat, residuals, rank)
```

## 6. 条件数与数值稳定性

矩阵二范数条件数为：

$$
\kappa(A)=\frac{\sigma_{\max}}{\sigma_{\min}}.
$$

条件数大意味着输入或舍入误差可能被明显放大。此时即使程序成功返回结果，结果也可能不可靠。

常见处理方式包括：

- 对数据进行缩放或归一化。
- 使用 QR 或 SVD 求解最小二乘。
- 加入正则化，例如岭回归 $(A^TA+\lambda I)\mathbf{x}=A^T\mathbf{b}$。
- 检查模型中是否存在冗余参数或不可观测方向。

## 7. 具身智能中的例子

- 使用多组对应点估计坐标变换。
- 使用传感器观测估计未知状态。
- 使用伪逆求冗余机械臂的关节速度。
- 在标定问题中拟合相机或机器人参数。

## 自测

1. 为什么 $\mathbf{b}$ 不在列空间时方程无解？
2. 最小二乘残差为什么与 $A$ 的每一列正交？
3. 为什么不推荐直接计算 $(A^TA)^{-1}$？

[上一节](01-vectors-and-matrices.md) · [下一节：特征值与矩阵分解](03-eigenvalues-and-decompositions.md)
