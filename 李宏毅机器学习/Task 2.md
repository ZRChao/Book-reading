
## Task 2 

5月17日22:00前，作业要求：https://shimo.im/docs/LrtvE7bsyewCWB14/read

学习打卡内容：
- 理解偏差和方差
  - 学习误差为什么是偏差和方差而产生的，并且推导数学公式
  - 过拟合，欠拟合，分别对应bias和variance什么情况
- 学习鞍点，复习上次任务学习的全局最优和局部最优
  - 解决办法有哪些
- 梯度下降
  - 学习Mini-Batch与SGD
  - 学习Batch与Mini-Batch，SGD梯度下降的区别
  - 如何根据样本大小选择哪个梯度下降(批量梯度下降，Mini-Batch）
  - 写出SGD和Mini-Batch的代码
- 学习交叉验证
- 学习归一化 
- 学习回归模型评价指标

## 偏差和方差

误差来源：随着模型复杂度的增大，虽然训练集上的误差能够降低，但是在测试集上误差却反而到最后增大。其误差来源主要是**偏差**和**方差**，
 - 偏差只拟合算法的期望预测与真实模型的偏离程度，刻画了算法本身的拟合能力,偏差大说明模型不够好拟合**underfitting**，可以通过增加其他有效参数进一步建模；另外一种策略便是学习若干个弱模型，然后整合结果，比如[bagging，boosting](https://mp.weixin.qq.com/s/3hwGAFIibddcSV-00mrSfg)等
 
 - 方差刻画了算法本身的复杂度，参数越多其可变性越高曲线越光滑，方差越大学习过度traindata本身的信息**overfitting**，增加数据样本量或是进行特征选择 **正则化**。

**公式证明**[参考南瓜书]() 
![](https://github.com/ZRChao/Book-reading/blob/master/李宏毅机器学习/figures/bias-variance.png)


![](https://github.com/ZRChao/Book-reading/blob/master/李宏毅机器学习/figures/bias-variance2.png)

 - 权衡偏差-方差，可以通过将train data切割一部分出来作为验证集validation， 首先通过traindata进行训练，然后通过验证集进行测试，确定模型，最后在用到测试数据中。如何切割，一种方式是hold out即只切割一部分出来，另外一种更普世的是**交叉验证 cross validation** (参见西瓜书)。
**需要说明的是，在kaggle类似的比赛中，往往由public的测试数据，我们可以通过上面的步骤训练好模型，然后预测public测试数据并计算误差，这也是我们比赛期间排名的依据，但是并不推荐用此测试数据来反过来修改我们的模型，因为最终排名还有一个private数据，很有可能你的模型在public中不是最优的，但是因为其一般性可以在private中达到最好。为此只需要对训练数据进行有效切割后训练模型，选择好模型后再用所有的测试数据拟合参数即可。**

[数学证明](参考西瓜书第二章如下) 
![](https://github.com/ZRChao/Book-reading/blob/master/李宏毅机器学习/figures/bias-variance3.png) 

## 梯度下降

在定义好损失函数或是目标函数进行极值求解过程中，可以通过梯度下降(或上升)求解，梯度也即是等高线的法线的方向。

![](https://github.com/ZRChao/Book-reading/blob/master/李宏毅机器学习/figures/GD1.png)

从梯度下降的式子可以看出，参数更新依赖三部分: **初始值**,**学习速率**以及**梯度大小**。
 - 当目标函数是凸函数(或凹函数时)，初始值的选择不会影响最终的结果，总会迭代到最优解。而当函数非凸(非凹)时，初始值的选择会导致局部最优，选择合适的初始值成为关键。

- 另外，当梯度变化很小时，算法也会停止更新，此时并未达到极值。可以看到初始值与梯度大小依赖于目标函数本身的性质，很难进行有效的选择与调节。如下图

![](https://github.com/ZRChao/Book-reading/blob/master/李宏毅机器学习/figures/GD2.png)

- 最后，对于一般的存在最优解的函数，学习速率成为调节的关键。当步长太短时，迭代收敛速度极慢；而当步长太长，则会导致跳过最优解。
    - 简单思想，开始步长长，后面不断减小
    - **Learning rate cannot be one-size-fits-all**

### Adagrad

$$w^{t+1} = w^t \frac{\eta}{\sqrt{\sum_{i=0}^t(g^i)^2}} g^t$$

![](https://github.com/ZRChao/Book-reading/blob/master/李宏毅机器学习/figures/GD3.png)

**没理解举得二次函数的例子解释，采用一阶微分除以二阶微分作为更新部分？**

### SGD stochastic gradient descent

比GD更快，因为每次更新参数，其随机选择一个样本计算梯度。

- 为了保证GD的收敛速度，更一般的可以选择一部分样本进行梯度的计算

### Feature scaling 

对不同的feature进行标准化，使得在不同参数的更新上朝着同一个方向趋于最优解。

![](https://github.com/ZRChao/Book-reading/blob/master/李宏毅机器学习/figures/GD4.png)

### 理论支持

在某一步参数更新时，目标函数在该处进行二阶泰勒展开，此时下一步最优化转化为找到对应的圆内的某点使得其与变化方向上的乘机最小。

![](https://github.com/ZRChao/Book-reading/blob/master/李宏毅机器学习/figures/GD5.png)

![](https://github.com/ZRChao/Book-reading/blob/master/李宏毅机器学习/figures/GD6.png)



## 模型评估

参见[西瓜书作业](https://github.com/ZRChao/Book-reading/tree/master/周志华西瓜书)
