# deeplearningnoteinkesci
伯禹教育平台深度学习笔记

# day1 
## 1.1 线性回归
## 线性回归的要素
### 模型 
这里可以简单理解为一个可以较好的表征特征值与预测值之间关系的函数表达式  
如***y = w1 * x1 + w2 * x2 +...+ b***  
其中**x1,x2**...表示特征1,特征2...;**w1,w2**...表示该特征对应的权重;**b** 即bias,偏执值
### 数据集
机器学习和深度学习中 我们通常按照作用不同把数据集分为三类:  
**训练集(Training set):**  用于训练模型,进行模型拟合的数据样本  
**验证集(Validation set):**  一般是模型训练中预留出来的一部分样本,用于对模型能力进行初步评估,并以此调整模型参数.  
**测试集(Test set):**  用来评估最终模型的数据集  
验证集和测试集的区别:  
![image](https://github.com/oak92/deeplearningnoteinkesci/blob/master/images/formulas/validationsetVStestset.png)
一个形象的比喻：  
    训练集-----------学生的课本；学生 根据课本里的内容来掌握知识。  
    验证集------------作业，通过作业可以知道 不同学生学习情况、进步的速度快慢。  
    测试集-----------考试，考的题是平常都没有见过，考察学生举一反三的能力。

传统上，一般三者切分的比例是：6：2：2，**验证集并不是必须的**。

**另外**,对于课程中指出 房屋的出售价格叫做标签(label),我个人不太赞成这样命名:  
通过建立模型来区分房屋好坏或者将房屋划分为三六九等,模型的输出是一个离散值,在确定决策边界之后,它一定属于某一类,这是一个**分类问题**. 打上对应类的标签是可以的.
而房屋价格预测,模型的输出是一个连续的值,是对真实值的一种逼近,本质上是一个**回归问题**.把输出值price打上标签像是在处理一个分类问题,不太妥当.  

### 损失函数
损失函数表示模型预测值与真实值之间的差距大小,损失函数越小,表示模型的拟合能力越强,预测或者分类越精准.  
我们可以将损失函数按照分类问题和回归问题分为两类:  
 
回归问题中的损失函数:  
1,均方误差(Mean Squared Error),也叫MSE/平方损失/L2损失. 
数学公式 
![image](https://github.com/oak92/deeplearningnoteinkesci/blob/master/images/formulas/MSE.png)
顾名思义，均方误差（MSE）度量的是预测值和实际观测值间差的平方的均值。它只考虑误差的平均大小，不考虑其方向。但由于经过平方，与真实值偏离较多的预测值会比偏离较少的预测值受到更为严重的惩罚。再加上 MSE 的数学特性很好，这使得计算梯度变得更容易。是回归问题中最常见的损失函数(本课程中使用的也是此函数).
  
2,均方根误差(Root Mean Squared Error),RMSE  
数学公式
![image](https://github.com/oak92/deeplearningnoteinkesci/blob/master/images/formulas/RMSE.png)

均方根误差是均方误差的算术平方根，能够直观观测预测值与实际值的离散程度。  
通常用来作为回归算法的性能指标。

3,平均绝对误差(Mean Absolute Error),MAE/L1损失  
数学公式
![image](https://github.com/oak92/deeplearningnoteinkesci/blob/master/images/formulas/MAE.png)
平均绝对误差（MAE）度量的是预测值和实际观测值之间绝对差之和的平均值。和 MSE 一样，这种度量方法也是在不考虑方向的情况下衡量误差大小。但和 MSE 的不同之处在于，MAE 需要像线性规划这样更复杂的工具来计算梯度。此外，MAE 对异常值更加稳健，因为它不使用平方。
平均绝对误差是绝对误差的平均值 ，平均绝对误差能更好地反映预测值误差的实际情况。  
通常用来作为回归算法的性能指标。  

分类问题中的损失函数:  
1,交叉熵损失(Cross Entropy Loss),也叫负对数似然  
这是分类问题中最常见的设置。随着预测概率偏离实际标签，交叉熵损失会逐渐增加。  
数学公式  
![image](https://github.com/oak92/deeplearningnoteinkesci/blob/master/images/formulas/CrossEntropy.png)
注意，当实际标签为 1(y(i)=1) 时，函数的后半部分消失，而当实际标签是为 0(y(i=0)) 时，函数的前半部分消失。简言之，我们只是把对真实值类别的实际预测概率的对数相乘。还有重要的一点是，交叉熵损失会重重惩罚那些置信度高但是错误的预测值。  
2,Hinge Loss/多分类 SVM 损失  
数学公式  
简言之，在一定的安全间隔内（通常是 1），正确类别的分数应高于所有错误类别的分数之和。因此 hinge loss 常用于最大间隔分类（maximum-margin classification），最常用的是支持向量机。尽管不可微，但它是一个凸函数，因此可以轻而易举地使用机器学习领域中常用的凸优化器。  

### 优化函数  
在利用损失函数（Loss Function）计算出模型的损失值之后，接下来需要利用损失值进行模型参数的优化。在实践操作最常用到的是一阶优化函数。包括GD，SGD，BGD，Adam等。一阶优化函数在优化过程中求解的是参数的一阶导数，这些一阶导数的值就是模型中参数的微调值。

本课程使用的是随机梯度下降(SGD) 需要注意的是对于SGD leaningRate(学习率)和batchSize(批量大小)直接决定了模型的参数更新  
学习率太大容易导致模型不收敛,太小容易导致导致收敛过慢或者无法学习 
学习率和批量大小如何影响模型性能可参考https://www.cnblogs.com/yumoye/p/11055813.html  
深度学习调参有哪些技巧？ - Captain Jack的回答 - 知乎  
https://www.zhihu.com/question/25097993/answer/127472322   
更多详细的优化函数的介绍可参见https://blog.csdn.net/shanglianlm/article/details/85019633  


## 模型搭建步骤  
简单来说 本课程中房价预测线性回归模型的搭建主要有以下7步、3个阶段:
### 第一阶段:数据准备阶段    
1,生成数据集  
2,读取数据集  
### 第二阶段:模型构建阶段  
3,定义模型  
4,初始化模型参数  
5,定义损失函数  
6,定义优化函数 
### 第三阶段:训练阶段 
7,开始训练  

这里没有体现构建和训练过程中的细节, 详情可参见https://mp.weixin.qq.com/s/zv8Mtch5Klm-qSgyBxI9QQ
## 1.2 Softmax与分类模型、多层感知机
