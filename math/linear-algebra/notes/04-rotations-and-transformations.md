# 04 旋转与刚体变换

## 1. 坐标与几何对象

几何对象本身不依赖坐标系，但它的坐标表示依赖所选择的基。机器人系统通常同时包含世界坐标系、机器人基座坐标系、末端坐标系和传感器坐标系，因此必须明确每个量属于哪个坐标系。

本文使用记号 ${}^{A}\mathbf{p}$ 表示点 $\mathbf{p}$ 在坐标系 $A$ 中的坐标，${}^{A}T_B$ 表示把坐标系 $B$ 中的坐标转换到坐标系 $A$。

## 2. 二维旋转

二维中逆时针旋转角度 $\theta$：

$$
R(\theta)=
\begin{bmatrix}
\cos\theta & -\sin\theta\\
\sin\theta & \cos\theta
\end{bmatrix}.
$$

旋转后的点为 $\mathbf{p}'=R\mathbf{p}$。连续旋转满足：

$$
R(\theta_1)R(\theta_2)=R(\theta_1+\theta_2).
$$

## 3. 三维旋转矩阵

三维旋转矩阵属于特殊正交群：

$$
SO(3)=\{R\in\mathbb{R}^{3\times3}\mid R^TR=I,\det(R)=1\}.
$$

因此：

$$
R^{-1}=R^T.
$$

绕 $z$ 轴旋转 $\theta$ 的矩阵为：

$$
R_z(\theta)=
\begin{bmatrix}
\cos\theta & -\sin\theta & 0\\
\sin\theta & \cos\theta & 0\\
0 & 0 & 1
\end{bmatrix}.
$$

三维旋转不可交换，一般有 $R_xR_y\neq R_yR_x$。组合旋转时必须明确主动/被动旋转、固定轴/运动轴以及乘法顺序。

## 4. 平移与齐次坐标

刚体变换同时包含旋转 $R$ 和平移 $\mathbf{t}$：

$$
\mathbf{p}'=R\mathbf{p}+\mathbf{t}.
$$

由于平移不是线性映射，引入齐次坐标：

$$
\tilde{\mathbf{p}}=
\begin{bmatrix}\mathbf{p}\\1\end{bmatrix},
\qquad
T=
\begin{bmatrix}
R & \mathbf{t}\\
\mathbf{0}^T & 1
\end{bmatrix}.
$$

于是刚体变换可以写成矩阵乘法：

$$
\tilde{\mathbf{p}}'=T\tilde{\mathbf{p}}.
$$

三维刚体变换集合称为 $SE(3)$。

## 5. 变换的组合与求逆

若已知 ${}^{A}T_B$ 和 ${}^{B}T_C$，则：

$$
{}^{A}T_C={}^{A}T_B{}^{B}T_C.
$$

矩阵相邻的坐标系标记应能“消去”，可用来检查乘法顺序。

刚体变换的逆为：

$$
T^{-1}=
\begin{bmatrix}
R^T & -R^T\mathbf{t}\\
\mathbf{0}^T & 1
\end{bmatrix}.
$$

注意逆变换的平移不是简单的 $-\mathbf{t}$，还必须先转换到逆变换的坐标轴中。

## 6. 点、方向和法向量

- **点**受旋转和平移影响，齐次坐标最后一维为 1。
- **方向向量**只受旋转影响，齐次坐标最后一维为 0。
- **法向量**在一般线性变换 $A$ 下应使用 $A^{-T}$ 变换；对于纯旋转，仍可直接使用 $R$。

区分这些对象可以避免把平移错误地应用于速度、力方向或表面法线。

## 7. NumPy 示例

```python
import numpy as np

theta = np.deg2rad(90.0)
R = np.array([
    [np.cos(theta), -np.sin(theta), 0.0],
    [np.sin(theta),  np.cos(theta), 0.0],
    [0.0,            0.0,           1.0],
])
t = np.array([1.0, 2.0, 0.0])

T = np.eye(4)
T[:3, :3] = R
T[:3, 3] = t

p_b = np.array([1.0, 0.0, 0.0, 1.0])
p_a = T @ p_b

assert np.allclose(R.T @ R, np.eye(3))
assert np.isclose(np.linalg.det(R), 1.0)
assert np.allclose(np.linalg.inv(T) @ p_a, p_b)
print(p_a)
```

## 8. 常见错误

- 未注明向量属于哪个坐标系。
- 把角度值直接传给需要弧度的函数。
- 混淆 $R_{AB}$ 与 $R_{BA}$。
- 按错误顺序组合矩阵。
- 将欧拉角直接相加来组合三维旋转。
- 数值累积后未检查 $R^TR=I$ 和 $\det(R)=1$。

## 自测

1. 为什么旋转矩阵的逆等于其转置？
2. 为什么齐次坐标能够把平移纳入矩阵乘法？
3. 已知 ${}^{W}T_B$ 和 ${}^{B}T_C$，如何得到 ${}^{W}T_C$？

[上一节](03-eigenvalues-and-decompositions.md) · [练习与自测](../exercises.md)
