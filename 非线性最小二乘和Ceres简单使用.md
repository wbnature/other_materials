# 非线性最小二乘和Ceres简单使用

[TOC]

## 一、非线性最小二乘简介

### 1.1 问题的描述

　　最小二乘问题可以描述如下 ：$$f(x)$$ 为一个函数，输入一个 $n$ 维的向量 $x$ 得到一个一维的数，最小二乘问题可以简单的解释为：找到一个函数关系，使得对于给定的 $(x, y)$ 拟合程度最高，或误差最小，一般当观测值多于变量个数时采用最小二乘方法进行误差分配，使得总体误差最小。 

![](1.png)

### 1.2 线性和非线性最小二乘

​	假如该问题为线性的，可以直接对目标函数求导，并且令其导数等于零，以此求得其极值，并通过比较求取全局最小值(Global Minimizer)，并将其最为目标函数的解。

​	但是如果问题为非线性，此时通常无法直接写出其导数形式（函数过于复杂），因此不再去试图直接找到全局最小值，而是退而求其次通过不停的迭代计算寻找到函数的局部最小值(Local Minimizer)，并认为该局部最小值能够使目标函数取得最优解（最小值），这就是非线性最小二乘的通常求解思路。

### 1.3 **非线性最小二乘相关定义** 

![](2.png)

​	如上图所示为一个典型的非线性最小二乘问题：得到所有离散点的最佳拟合曲线，而曲线的方程给定如下：

$$
M(x,t) = x_3e^{x_1t} + x_4e^{x_2t}
$$
​	其中 $x$ 为要求的参数，$t$ 为自变量。根据函数关系可知，此函数有四个参数，分别为$x_1$，$x_2$，$x_3$，$x_4$，则假设有如下关系：
$$
y_i = M(x^r,t_i) + \epsilon_i
$$
​	其中 $y_i$ 与 $x^r$ 是给定的真值，$\epsilon_i$ 为误差通过函数 M 拟合之后的误差项，要获取最佳的拟合曲线，则是要使得误差项最小, 即：
$$
\begin{align}f_i(x) = & y_i - M(x,t_i)\\   
                    = & y_i -  x_3e^{x_1t} - x_4e^{x_2t} , i = 1,...,m.
\end{align}
$$

​	以上所描述的问题就是一个典型的非线性最小二乘的问题。 对于上述这个具体问题，可以将其变得更加抽象化，同时定义全局最小值满足以下定义： 

![](3.png)

​	通常来说求全局最小值的的过程比较复杂，且难以实现，因此在实际使用的过程中会讨论一些比较简单的问题，即讨论函数 $F$ 的局部最小值，定义局部最小值满足以下定义：

![](4.png)

 	相对于全局最小值问题而言，局部最小值通过一个极小值区间的约束将问题大大简化了。（对于全局最小值来说，不存在数值解法能够辨理整个区间进行求解，通常的迭代方法会陷入局部最小值陷阱，可能没办法得到真正意义上的全局最小值，当然如果能够证明函数在定义域上存在唯一的极值，则可以通过局部最小值的迭代算法求出其唯一极值作为全局的最小值） 

​	对于代价函数 $F$，假定 $F$ 是可微的，将 $F$ 进行二阶泰勒展开可得：
$$
F(x + h) = F(x) + h^Tg + \frac 12 h^THh  + O(||h||^3)
$$
​	其中 $g$ 为 $F$ 的一阶偏导，$H$ 为 $F$ 的二阶偏导矩阵。

​	假设 $x^*$ 为局部最小值，则即使 $||h||$ 足够小，也无法找到一个点 $x + x^*$, 使得其对应的函数值小于 $x^*$ 处的函数值。因此可得到以下结论：

![](5.png)

​	同时也将所有满足令函数在其处一阶导数等于零的点为驻点(Stationary Point)。

![](6.png)

​       综上所述，可知局部最小值为函数的驻点，但是驻点同时也有肯能为函数的局部最大值或者鞍点(saddle point)。因此为区分某一个点到底是局部最小值、局部最大值或者是鞍点，我们在需要保留泰勒展开式的二阶信息。通过判定展开式中的 $H$(*Hessian*) 矩阵的相关性质进行判定：

![](7.png)



## 二、下降法

​	几乎所有求解非线性优化的方法都是从一个函数的初始值 $x_0$ 开始求解得到一些列的值$(x_1, x_2,...)$，并希望最后函数收敛于局部最小值 $x^*$。因此，可以得到函数的下降条件为：
$$
F(x_{k+1}) < F(x_k)
$$
​       该条件可以使得函数最后不收敛于局部最大值，并且可以在一定程度上使函数尽可能小概率的收敛于鞍点。

### 2.1 下降法计算流程

​	下降法满足上面所提到的条件，且每一步迭代中都主要包括两部分内容，分别是求解下降方向 $h_d$ 和合理的步长 $\alpha$。下降法的整体流程如下:

![](21.png)

​	该流程从给定的初始值开始，在每一步迭代中首先计算下降的方向，然后在计算合理的步长，步长计算采用line search 方法。最后根据下降方向和步长产生 $\Delta x$，并与此次迭代的初始值 $x$ 相加得到下次迭代的初始值 $x^´$。以此类推直到最后找到收敛值后，停止计算。

### 2.2 最速下降法(梯度法)

​	对函数 $F(x + \alpha h)$ 进行泰勒展开，当 $\alpha$ 足够小时得到：
$$
\begin{align}F(x + \alpha h) = & F(x) + \alpha h^TF^´(x) + O(\alpha^2)\\   
                    ≈ & F(x) + \alpha h^TF^´(x)  
\end{align}
$$

​	其相对增益函数为：

![](22.png)

​	角度 $\theta$ 为 $h$ 和 $F$ 一阶导数之间的向量夹角。如果 $\theta = \pi$ 就可以得到最快下降方向 $h_{sd} = -F^´(x)  $。

​	最速下降法是最简单的方法，但这种方法还存在这比较大的问题：

* 步长的选择，由于是给定步长，在距离极小值比较远的初始点收敛比较慢，而距离极值点比较近的位置有			  会错过极值点而产生震荡现象。
* 需要对各个方向进行尝试，运算次数较多，运算复杂。

### 2.3 牛顿法

​	根据定义 $F^´(x^*) = 0$ 可推导牛顿法：

![](23.png)

​	由于其一阶导数为 0，则有如下等式：

![](24.png)

​	则下一次的迭代位置为：
$$
x : = x + h_n
$$
​	可以将牛顿迭代法和梯度下降法结合起来，即：

![](C:\Users\Nature\Desktop\非线性最小二乘\25.png)

​	牛顿法每一步都需要求解目标函数的 Hessian 矩阵的逆矩阵，计算比较复杂。 



## 三、非线性最小二乘问题

​	非线性最小二乘问题目的为求解代价函数的最优解（局部最小值）使得函数的值最小：
$$
x^* = argmin_x\{F(x)\}
$$
 	此时

![](31.png)

​       上式中，函数 $F(x)$ 为需要求解的代价函数（目标函数），$$f(x)$$ 是由模型函数与测量值之差构成，$x^*$为代价函数的局部最小值。

### 3.1 高斯牛顿法

​	高斯牛顿法是一种求解非线性最小二乘的简单算法，该算法的基本思想是将函数非线性 $f(x)$ 进行一阶泰勒展开（**注意此处展开的是函数 $f(x)$ 而非代价函数 $F(x)$**）：
$$
f(x + h) = f(x) + J(x)h + O(||h||^2)
$$
​	此处 $J(x)$ 为函数 $f(x)$ 的雅克比矩阵，即函数 $f(x)$ 的一阶导数。

​	根据前面所述最小二乘的目的其实就是求解合适的变量值 $\Delta x$ 使得函数的值 $||f(x + \Delta x)||^2$ 达到最小。则

![](32.png)

​	式中，$f = f(x), J = J(x).$
$$
L^´(h) = J^Th + J^TJh\\
L^{ "}(h) = J^TJ
$$
​	则高斯牛顿法的下降方向可得到：
$$
(J^TJ)h_{gn} = -J^Tf
$$
​	则高斯牛顿法更新步骤为：

![](33.png)

​	上式是一个线性方程组，称之为增量方程，高斯牛顿方程或正规方程（Normal equation）。

​	正规方程的求解通常是采用QR 分解、乔姆斯基分解(Cholesky decomposition)和奇异值分解(SVD)等方法求解。

​	和牛顿法相比，此处的 $J^TJ$ 矩阵其实是 $H$(Hessian) 矩阵的近似，省去了 $H$ 矩阵的计算。

​	根据前面所提到的，为了保证所求解的 $x$ 为函数的局部最小值，需要保证 $J^TJ$ 矩阵为正定的，但是在实际中 $J^TJ$ 矩阵很有可能为奇异矩阵或者病态的，此时增量的稳定性较差，导致算法不收敛。更严重的是，假设 $J^TJ$ 矩阵非奇异也非病态，如果所用的步长太大，会使 $f(x+h)$ 的近似不够准确，这样也不能保证算法的收敛性。

### 3.2 列文伯格-马夸尓特方法（L-M方法, 阻尼牛顿法）

​	L-M方法是结合高斯牛顿法和最速下降法的一种方法在, L-M方法中，所求解的方程满足：
$$
min\frac12||f(x) + J(x)||^2, s.t.||Dh_{lm}||^2 ≤\Delta
$$
​	式中 $\Delta$ 是信赖区域的半径，$D$ 控制信赖区域的形状。

​	用拉格朗日乘子将上式转换为一个无约束问题，即
$$
min\frac12||f(x) + J(x)h_{lm}||^2 +\frac\mu 2||Dh_{lm}||^2
$$
​	式中 $\mu$ 是拉格朗日乘子，其增量方程为：
$$
(J^TJ + \mu D^TD)h_{lm} = -J^Tf
$$
​	当 $D = I,$ 即信赖区域为球形时，上式化为：
$$
(J^TJ + \mu I)h_{lm} = -J^Tf
$$
​	从上式看到，当 $\mu$ 比较小时，$J^TJ$ 占主导地位，L-M法更接近于高斯牛顿法；当 $\mu$ 比较大时，$\mu I$ 占主导地位，L-M法更接近于最速下降法。

​	在L-M法中信赖区域大小的确定是用增益比例来进行判定的，即：

![](C:/Users/Nature/Desktop/%E9%9D%9E%E7%BA%BF%E6%80%A7%E6%9C%80%E5%B0%8F%E4%BA%8C%E4%B9%98/34.png)

​	信赖区域的更新使用下面的方法：

![](C:/Users/Nature/Desktop/%E9%9D%9E%E7%BA%BF%E6%80%A7%E6%9C%80%E5%B0%8F%E4%BA%8C%E4%B9%98/35.png)

​	如果增益比例过小（例如小于0.25），说明实际改变的量远远小于近似模型的值，此时近似情况并不理想，因此需要缩小近似范围。反之如果比例较大，说明实际下降的比预期的更大，近似情况比较好，可以适当放大一下近似范围。

### 3.3 Dog-Leg 方法

​	Dog-Leg 方法也是结合高斯牛顿法和最速下降法的一种方法，它也是信赖区域法的一种。
- 对于高斯牛顿(Gauss-Newton)法，$h_{gn}$ 通过下式求解：

$$
(J(x)^TJ(x))h_{gn} = -J(x)^Tf(x)
$$

- 对于最速下降法(steepest descent)，$h_{sd}$ 通过下式求解：

$$
h_{sd} = -g = -J(x)^Tf(x)
$$

​	需要注意的是 $h_{sd}$ 实际上是一个方向，而不是实际走的一步，实际走的是 $\alpha h_{sd}$ ,下面说明如何求解 $\alpha$，对于原始的  $F$ :

![](36.png)

​	要使得  $F$ 获取最小值，可以对上式关于 $\alpha$ 求导，令其为０，可以得到：

![](37.png)

​	现在对于每一步，都有两种可以选择的方式：一种是利用最速下降计算的更新，记为：$\alpha = \alpha h_{sd}$；另一种是利用高斯牛顿法获取的更新，记为 $b = h_{gn}$，Dog-Leg 方法使用以下的策略进行选择，假定信赖区域半径 $\Delta$ ：

![](38.png)

![](311.png)

​	其中$\beta$ 的计算方法为：
$$
c = a^T(b-a)
$$
![](39.png)

![](310.png)

## 四、Ceres简单使用

### 4.1 求解激光传感器位置

​	待求参数激光传感器位置 $(x^*, y^*)$, 已知反光板的位置 $(x, y)$，激光传感器和反光板之间的距离和角度 $(d, \theta)$ .

​	误差模型：
$$
e(x, y) = d^2 - (x^* - x)^2 - (y^*-y)^2
$$

```c++
struct CostFunctor   // 代价函数的计算模型，仿函数
{
    CostFunctor(double x , double y , double d) : _x(x), _y(y), _d(d) {}
    template <typename T>    // 计算残差
    bool operator()(const T* const params , T* residual)const
    {
        T dx = params[0] - T(_x);
        T dy = params[1] - T(_y);
        residual[0] = T(_d) * T(_d) - dx * dx - dy * dy; // d^2 - (dx^2 + dy^2)
        //residual[0] = _d - ceres::sqrt(dx * dx + dy * dy); // d - sqrt(dx^2 + dy^2)
        return true;
    }
    const double _x, _y, _d;
};

bool mapping::calculateLaserPoseWithCeres(std::vector<index_pair_> &pairs , std::vector<struct polar_coordinate_> &reflectors)
{
    double params[2] = {laserPose.x , laserPose.y}; // 待优化参数
    // 第一步，构建最小二乘问题
    ceres::Problem problem; 
	double x0, y0, d0;
    for(auto iter=pairs.begin(); iter!=pairs.end();++iter)
    {
		auto findIndex = gcMap.find((*iter).mapIndex);
		if(findIndex != gcMap.end())
		{
			x0 = findIndex->second.x_m;
			y0 = findIndex->second.y_m;
		}
		else
			return false;
		
		d0 = reflectors[(*iter).scanSetIndex].distance_m;
      //利用之前写的结构体、仿函数，创建代价函数对象，注意初始化的方式
      //尖括号中的参数分别为误差类型，输出维度(因变量个数)，输入维度(待估计参数的个数)
        ceres::CostFunction* cost_function = new ceres::AutoDiffCostFunction<CostFunctor,1,2>(new CostFunctor(x0 , y0 , d0));
      //三个参数分别为代价函数、核函数和待估参数
        problem.AddResidualBlock(cost_function,NULL,params);
    }

    //第二步，配置Solver
    ceres::Solver::Options options;
    //配置增量方程的解法
    options.linear_solver_type=ceres::DENSE_QR;
    //是否输出到cout
    options.minimizer_progress_to_stdout=false;
    //第三步，创建Summary对象用于输出迭代结果
    ceres::Solver::Summary summary;
    //第四步，执行求解
    ceres::Solve(options,&problem,&summary);
    //第五步，输出求解结果
    //std::cout<<summary.BriefReport()<<std::endl;
    laserPose.x = params[0];
    laserPose.y = params[1];
    
	......
}
```

**class Solver::Options 类：**

~~~c++
Solver::Options::minimizer_type
Default: TRUST_REGION 
在LINE_SEARCH和TRUST_REGION算法之间进行选择。

Solver::Options::line_search_direction_type
Default: TRUST_REGION 
在LINE_SEARCH和TRUST_REGION算法之间进行选择。

Solver::Options::line_search_interpolation_type
Default: CUBIC 
用于近似目标函数的多项式的次数。可选 BISECTION, QUADRATIC 和 CUBIC.

Solver::Options::max_num_line_search_step_size_iterations
Default: 20 
在每次线性搜索中，最大的寻找步长的迭代次数，如果在这个数量的试验中不能找到满足条件的步长，那么线性搜索就会停止。

Solver::Options::max_num_line_search_direction_restarts
Default: 5 
在终止优化之前，线搜索方向算法的最大重启次数。当当前算法不能产生新的下降方向时，线搜索方向算法重新启动。这通常表示一个数值失效，或者是使用近似失效。

Solver::Options::trust_region_strategy_type
Default: LEVENBERG_MARQUARDT 
Ceres使用的置信域计算方法，当前可选LEVENBERG_MARQUARDT和DOGLEG。

Solver::Options::dogleg_type
Default: TRADITIONAL_DOGLEG 
Ceres支持两种dogleg策略，Powell的TRADITIONAL_DOGLEG方法和 ByrdSchnabel]描述的SUBSPACE_DOGLEG 方法。

Solver::Options::max_num_iterations
Default: 50 
解析器应该运行的最大迭代次数。

Solver::Options::max_solver_time_in_seconds
Default: 1e6 
解析器应该运行的最大时间。

Solver::Options::num_threads
Default: 1 
Ceres求Jacobain的线程数目。

Solver::Options::initial_trust_region_radius
Default: 1e4 
初始置信域的大小。当使用levenbergmarquardt策略时，这个数字的倒数就是初始的正则化参数。

Solver::Options::max_trust_region_radius
Default: 1e16 
置信域的半径是不允许超过这个值的。

Solver::Options::min_trust_region_radius
Default: 1e-32 
置信域的半径是不允许小于这个值的。

Solver::Options::linear_solver_type
Default: SPARSE_NORMAL_CHOLESKY / DENSE_QR 
在 Levenberg-Marquardt算法的每一次迭代中，用于计算线性最小二乘问题的线性求解器的类型。如果Ceres是支持西SuiteSparse，或者 CXSparse，或者是Eigen的稀疏Cholesky 分解，默认是 SPARSE_NORMAL_CHOLESKY，否则就是DENSE_QR。
~~~

### 4.2 求解鲍威尔方程

​	一个更复杂一点的例子，最小化 Powell 方程。令 $x=[x_1, x_2, x_3, x_4]$，并且  
$$
\begin{align}f_1(x) = & x_1 + 10x_2 \\   
                       f_2(x) = & \sqrt5 (x_3 - x_4) \\
                       f_3(x) = &  (x_2 - 2x_3)^2 \\
                       f_4(x) = & \sqrt{10} (x_1 - x_4)^2\\
                       F(x) = & [f_1(x), f_2(x), f_3(x), f_4(x)]
\end{align}
$$
​	其中 $F(x)$ 是一个有四个参数的方程，有四个残差，目的是找到一个 $x$，使得 $\frac12 ||F(x)||^2$ 最小。 

~~~c++
using ceres::AutoDiffCostFunction;
using ceres::CostFunction;
using ceres::Problem;
using ceres::Solver;
using ceres::Solve;

struct F1 {
    template <typename T>
            bool operator()(const T* const x1, const T* const x2, T* residual) const {
        residual[0] = x1[0] + T(10.0)*x2[0];
        return true;
    }
};

struct F2 {
    template <typename T>
            bool operator()(const T* const x3, const T* const x4, T* residual) const {
        residual[0] = T(sqrt(5.0))*(x3[0]-x4[0]);
        return true;
    }
};

struct F3 {
    template <typename T>
            bool operator()(const T* const x2, const T* const x3, T* residual) const {
        residual[0] = (x2[0]-T(2.0)*x3[0])*(x2[0]-T(2.0)*x3[0]);
        return true;
    }
};

struct F4 {
    template <typename T>
            bool operator()(const T* const x1, const T* const x4, T* residual) const {
        residual[0] = T(sqrt(10.0))*(x1[0]-x4[0])*(x1[0]-x4[0]);
        return true;
    }
};

int main(int argc, char** argv) {
//    CERES_GFLAGS_NAMESPACE::ParseCommandLineFlags(&argc, &argv, true);
    google::InitGoogleLogging(argv[0]);

    double x1 =  3.0;
    double x2 = -1.0;
    double x3 = 0.0;
    double x4 = 1.0;

    // 建立Problem
    Problem problem;

    // 设置四个残差方程Fx，用自动求导获得导数（或者雅克比矩阵）
    // 自动求导的模板参数<误差类型， 输出维度， 输入维度>，如果输入有多个参数，就依次展开
    CostFunction* cost_function1 = new AutoDiffCostFunction<F1, 1, 1, 1>(new F1);
    CostFunction* cost_function2 = new AutoDiffCostFunction<F2, 1, 1, 1>(new F2);
    CostFunction* cost_function3 = new AutoDiffCostFunction<F3, 1, 1, 1>(new F3);
    CostFunction* cost_function4 = new AutoDiffCostFunction<F4, 1, 1, 1>(new F4);

    // 设置残差模块，参数（代价函数，核函数，待估计参数）
    problem.AddResidualBlock(cost_function1, NULL, &x1, &x2);
    problem.AddResidualBlock(cost_function2, NULL, &x3, &x4);
    problem.AddResidualBlock(cost_function3, NULL, &x2, &x3);
    problem.AddResidualBlock(cost_function4, NULL, &x1, &x4);

    // 设置求解器选项
    Solver::Options options;
    options.max_num_iterations = 100;
    options.linear_solver_type = ceres::DENSE_QR;   //增量方程的求解方式
    options.minimizer_progress_to_stdout = true;    //是否把最小二乘的优化过程输出到cout

    cout << "Initial x1 = "<< x1 << ", x2 = " << x2 << ", x3 = " << x3 << ", x4 = " << x4 << endl;

    Solver::Summary summary;    //优化信息
    Solve(options, &problem, &summary); //开始优化

    cout << summary.BriefReport() << endl;  //简要报告
    cout << summary.FullReport() << endl;   //完整报告
    cout << "Final x1 = " << x1 << ", x2 = " << x2 << ", x3 = " << x3 << ", x4 = " << x4 << endl;

    return 0;
}
~~~



## 参考文献

1. [SLAM中的优化理论（一）—— 线性最小二乘](https://www.cnblogs.com/leexiaoming/p/7224781.html) 
2. [SLAM中的优化理论（二）—— 非线性最小二乘](https://www.cnblogs.com/leexiaoming/p/7257198.html) 
3. [非线性最小二乘学习（Nonline Least Square）](http://blog.sina.com.cn/s/blog_a29eae2b0102whjp.html) 
4. [Dog-Leg 最小二乘优化](https://zhuanlan.zhihu.com/p/42523077)
5. [Ceres 官网](http://www.ceres-solver.org/)
6. [ Ceres-Solver学习笔记](https://blog.csdn.net/huajun998/article/category/6788157) 
7. [Ceres Solver 官方教程学习笔记](https://blog.csdn.net/wzheng92/article/category/7496047) 
