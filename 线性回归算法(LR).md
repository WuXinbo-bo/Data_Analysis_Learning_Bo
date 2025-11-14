<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

# 回归模型：优化方法（路径）——最小二乘法


## 一、核心逻辑
利用**微积分极值条件**（函数在最小值点的偏导数为0），将“最小化残差平方和”的优化问题转化为解线性方程组，直接得到解析解。


## 二、步骤推导（以一元线性回归为例）
一元线性回归的模型表达式：
$$ y = \theta_0 + \theta_1 x + \epsilon $$
其中：
- \( y \)：因变量（待预测值）
- \( x \)：自变量（特征）
- \( \theta_0 \)：截距项（偏置）
- \( \theta_1 \)：斜率项（特征系数）
- \( \epsilon \)：误差项


### 目标函数
最小化**残差平方和**（观测值与预测值的误差平方和）：
$$ S = \sum_{i=1}^n (y_i - \hat{y}_i)^2 = \sum_{i=1}^n (y_i - \theta_0 - \theta_1 x_i)^2 $$
其中 \( \hat{y}_i = \theta_0 + \theta_1 x_i \) 是第 \( i \) 个样本的预测值。


### Step 1：对 \( \theta_0 \) 求偏导并令其为0
对目标函数 \( S \) 关于 \( \theta_0 \) 求偏导：
$$
\frac{\partial S}{\partial \theta_0} = -2 \sum_{i=1}^n (y_i - \theta_0 - \theta_1 x_i) = 0
$$

两边除以 \(-2\) 后化简，得到**正规方程1**：
$$
\sum_{i=1}^n y_i = n\theta_0 + \theta_1 \sum_{i=1}^n x_i
$$


### Step 2：对 \( \theta_1 \) 求偏导并令其为0
对目标函数 \( S \) 关于 \( \theta_1 \) 求偏导：
$$
\frac{\partial S}{\partial \theta_1} = -2 \sum_{i=1}^n x_i \cdot (y_i - \theta_0 - \theta_1 x_i) = 0
$$

两边除以 \(-2\) 后化简，得到**正规方程2**：
$$
\sum_{i=1}^n x_i y_i = \theta_0 \sum_{i=1}^n x_i + \theta_1 \sum_{i=1}^n x_i^2
$$


### Step 3：解正规方程组
联立两个正规方程，求解 \( \theta_0^* \)（最优截距）和 \( \theta_1^* \)（最优斜率）：

#### （1）求解 \( \theta_1^* \)
将正规方程1变形为 \( \theta_0 = \frac{1}{n}\sum_{i=1}^n y_i - \theta_1 \cdot \frac{1}{n}\sum_{i=1}^n x_i \)，代入正规方程2，化简得：
$$
\theta_1^* = \frac{n \cdot \sum_{i=1}^n x_i y_i - \left( \sum_{i=1}^n x_i \right) \cdot \left( \sum_{i=1}^n y_i \right)}{n \cdot \sum_{i=1}^n x_i^2 - \left( \sum_{i=1}^n x_i \right)^2}
$$


#### （2）求解 \( \theta_0^* \)
利用样本均值 \( \bar{x} = \frac{1}{n}\sum_{i=1}^n x_i \)、\( \bar{y} = \frac{1}{n}\sum_{i=1}^n y_i \)，代入 \( \theta_0 = \bar{y} - \theta_1 \bar{x} \) 得：
$$
\theta_0^* = \bar{y} - \theta_1^* \cdot \bar{x}
$$


## 三、数值示例（验证）
假设样本数据：
| \( x_i \) | 1 | 2 | 3 | 4 | 5 |
|----------|---|---|---|---|---|
| \( y_i \) | 2 | 4 | 5 | 4 | 6 |


### 计算统计量
- \( n = 5 \)
- \( \sum x_i = 1+2+3+4+5 = 15 \)
- \( \sum y_i = 2+4+5+4+6 = 21 \)
- \( \sum x_i y_i = 1×2 + 2×4 + 3×5 + 4×4 + 5×6 = 69 \)
- \( \sum x_i^2 = 1^2+2^2+3^2+4^2+5^2 = 55 \)


### 计算最优参数
$$
\theta_1^* = \frac{5×69 - 15×21}{5×55 - 15^2} = \frac{345 - 315}{275 - 225} = \frac{30}{50} = 0.6
$$

$$
\theta_0^* = \frac{21}{5} - 0.6×\frac{15}{5} = 4.2 - 1.8 = 2.4
$$


最终回归模型：\( \hat{y} = 2.4 + 0.6x \)
