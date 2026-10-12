# 02 二阶方法与非线性最小二乘

## 1. Newton 法

对二阶 Taylor 模型求极小得到：

$$
H_k\Delta x=-g_k,
\qquad x_{k+1}=x_k+\Delta x.
$$

接近非退化极小点时可二次收敛，但远离解时海森矩阵可能不正定，Newton 方向甚至不是下降方向。

## 2. 线搜索与信赖域

线搜索先选方向再选步长；信赖域在局部区域内最小化模型：

$$
\min_{\lVert p\rVert\le\Delta}m_k(p).
$$

实际下降与预测下降之比决定接受步骤及调整半径。信赖域通常比裸 Newton 更稳健。

## 3. 拟 Newton

BFGS 用梯度变化更新海森逆近似，并在合适条件下保持正定。L-BFGS 只保存最近若干更新，适合高维问题。

## 4. 非线性最小二乘

$$
F(x)=\frac12r(x)^Tr(x).
$$

梯度与精确海森：

$$
g=J^Tr,
\qquad H=J^TJ+\sum_ir_i\nabla^2r_i.
$$

Gauss–Newton 忽略第二项，残差小或模型接近线性时近似良好。

## 5. Levenberg–Marquardt

$$
(J^TJ+\lambda D)\Delta=-J^Tr.
$$

$\lambda$ 大时接近小步梯度下降，小时时接近 Gauss–Newton。应根据步骤效果动态调整，而非固定不变。

## 6. 鲁棒损失

平方损失对离群点敏感。Huber、Cauchy 等鲁棒核降低大残差影响：

$$
\min_x\sum_i\rho(\lVert r_i(x)\rVert^2).
$$

鲁棒核不能替代数据质量检查，也可能掩盖系统性建模错误。

## 7. 稀疏结构

SLAM、Bundle Adjustment 和轨迹优化的残差只依赖少量变量，雅可比高度稀疏。利用稀疏分解、变量消元和 Schur complement 是规模化求解的关键。

## 8. 导数检查

比较解析方向导数与中心差分，并随着 $\epsilon$ 变化观察误差先下降后因舍入上升。导数错误常表现为线搜索反复缩步或 LM 阻尼持续增大。

## 自测

1. Newton 法为何可能走向上升方向？
2. Gauss–Newton 忽略了海森的哪一部分？
3. LM 阻尼大时为什么更像梯度下降？
4. 鲁棒核为何不能修复错误数据关联？

[上一节](01-convexity-and-first-order-methods.md) · [下一节](03-constrained-and-robotics-optimization.md)
