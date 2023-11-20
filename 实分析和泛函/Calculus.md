# 2D



##  **平面曲线弧长公式**

1. **直角坐标。**设函数$f(x)$在区间$(a,b)$内具有连续导数，则曲线$y = f(x)$的弧微分公式为$\mathrm{d}s = \sqrt{1+y^{'}} \mathrm{d}x$。弧长为$s = \int_a^b \sqrt{1+y^{'}} \mathrm{d}x$。

2. **参数方程**。设曲线由参数方程
   $$
   ~
   \begin{cases}
   x = \varphi(t),  \\
   y = \psi(t),
   \end{cases}
   ~~~~~~~(\alpha \leq t \leq \beta)
   $$
   因为
   $$
   \Delta x = \varphi(t+ \mathrm{d}t) - \varphi(t) \approx \mathrm{d}x = \varphi^{'}(t) \mathrm{d}t, \\
   \Delta y = \psi(t+ \mathrm{d}t) - \psi(t) \approx \mathrm{d}y = \psi^{'}(t) \mathrm{d}t,
   $$
   所以，$\Delta s$的近似值(弧微分)即弧长元素为
   $$
   \mathrm{d}s = \sqrt{(\mathrm{d}x)^2 + (\mathrm{d}y)^2} = \sqrt{ {\varphi^{'}}^2(t)(\mathrm{d}t)^2 + {\psi^{'}}^2(t)(\mathrm{d}t)^2 } \\
    = \sqrt{{\varphi^{'}}^2(t)  + {\psi^{'}}^2(t) } \mathrm{d}t
   $$
   于是所求弧长为$s = \int_a^b \sqrt{{\varphi^{'}}^2(t)  + {\psi^{'}}^2(t) } \mathrm{d}t$。

3. **极坐标方程**

   当曲线坐标由极坐标方程
   $$
   \rho = \rho(\theta)~~~~(\alpha \leq \theta \leq \beta)
   $$
   给出，其中$\rho(\theta)$在$[\alpha, \beta]$上具有连续导师，则由直角坐标与极坐标的关系可得，
   $$
   ~
   \begin{cases}
   x = x(\theta) = \rho(\theta)\cos(\theta),  \\
   y = y(\theta) = \rho(\theta)\sin(\theta),
   \end{cases}
   ~~~~~~~(\alpha \leq \theta \leq \beta)
   $$
   这就是以极角$\theta$为参数的曲线弧的参数方程，于是弧长元素为
   $$
   \mathrm{d}s = \sqrt{ {x^{'}}^{2}(\theta) + {y^{'}}^{2}(\theta)} \mathrm{d}\theta = \sqrt{ \rho^2(\theta) + {\rho^{'}}^2(\theta) } \mathrm{d}\theta
   $$
   从而弧长为
   $$
   s = \int_{\alpha}^{\beta} \sqrt{ \rho^2(\theta) + {\rho^{'}}^2(\theta) } \mathrm{d}\theta
   $$

## **平面图形的面积：**

+ 直角坐标情形

+ 极坐标情形

  设由曲线$\rho  = \rho(\theta)$及射线$\theta = \alpha, \theta = \beta$围成一个图形(简称为曲边扇形)，则面积微元$\mathrm{d}S = \frac{1}{2}[\rho(\theta)]^2 \mathrm{d}\theta$，则曲边扇形的面积为$ S = \int_{\alpha}^{\beta} \frac{1}{2}[\rho(\theta)]^2 \mathrm{d}\theta$。



#  空间直线、平面、曲面



## 空间直线及其方程



## 空间曲面及其方程



## 空间曲线及其方程



## 空间曲线的切线与法平面



## 曲面的切平面与法线





## 方向导数和梯度



























 







# 重积分

## 二重积分



## 三重积分









#  曲线积分、曲面积分、体积分



## 





































