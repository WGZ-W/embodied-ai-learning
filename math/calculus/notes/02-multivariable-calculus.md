# 02 多元微分、局部近似与误差传播

## 1. 偏导与全微分

偏导只改变一个变量。全微分汇总所有一阶变化：

$$
df=\sum_i\frac{\partial f}{\partial x_i}dx_i
=\nabla f^Td\mathbf{x}.
$$

梯度依赖坐标和内积约定。在欧氏空间中，它是使方向导数最大的单位方向。

## 2. 多元链式法则

若 $\mathbf{y}=g(\mathbf{x})$、$\mathbf{z}=f(\mathbf{y})$，则：

$$
J_{f\circ g}(\mathbf{x})=J_f(\mathbf{y})J_g(\mathbf{x}).
$$

形状检查非常重要：若 $g:\mathbb{R}^n\to\mathbb{R}^m$、$f:\mathbb{R}^m\to\mathbb{R}^p$，最终雅可比为 $p\times n$。

## 3. 雅可比的几何意义

在工作点附近：

$$
f(\mathbf{x}+\Delta\mathbf{x})
\approx f(\mathbf{x})+J\Delta\mathbf{x}.
$$

雅可比把输入空间中的小位移映射为输出空间中的小位移。奇异值描述不同输入方向被放大或缩小的程度；接近零的奇异值对应局部不可观测或难以控制方向。

## 4. 海森矩阵

$$
H=\nabla^2f,
\qquad
f(x+\Delta)\approx f(x)+g^T\Delta+rac12\Delta^TH\Delta.
$$

海森矩阵刻画梯度如何变化。特征值给出各主方向的曲率；条件数大意味着不同方向尺度差异大，梯度下降可能缓慢。

## 5. 多元 Taylor 展开

一阶展开适合线性化，二阶展开适合局部优化。展开点必须明确，且只在足够小邻域内可靠。

对复合系统 $y=h(f(x))$，可以逐层计算局部雅可比并相乘，避免直接展开复杂表达式。

## 6. 变量变换与积分

多重积分变量变换需要 Jacobian 行列式：

$$
\int_{\Omega_y}f(y)dy
=\int_{\Omega_x}f(g(x))|\det J_g(x)|dx.
$$

它描述局部体积缩放。概率密度变换、极坐标积分和流模型都依赖这一项。

## 7. 一阶误差传播

若随机变量 $x$ 均值为 $\mu$、协方差为 $\Sigma_x$，且 $y=f(x)$，在 $\mu$ 处线性化：

$$
\Sigma_y\approx J\Sigma_xJ^T.
$$

这是 EKF 和传感器不确定性传播的基本公式。非线性强或分布很宽时，一阶近似可能失真。

## 8. 机器人雅可比

对末端位姿 $x=f(q)$：

$$
\dot x=J(q)\dot q.
$$

若 $J$ 降秩，机器人处于奇异构型，某些笛卡尔速度无法产生或需要极大关节速度。使用伪逆时常加入阻尼减小奇异附近的不稳定。

## 自测

1. 雅可比相乘的顺序如何由维度确定？
2. 海森矩阵的负特征值说明什么？
3. 为什么概率密度变换需要绝对 Jacobian 行列式？
4. 一阶协方差传播何时可能不可靠？

[上一节](01-single-variable-calculus.md) · [下一节](03-matrix-calculus-and-autodiff.md)
