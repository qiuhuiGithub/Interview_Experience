## 一面

1.介绍自己：略
2.介绍项目
    (1)指标为啥是这么定义的
    (2)直接把坐标给到神经网络，能学习到空间关系吗？
    (3)模型升级，指标提升却不多，怎么看这个问题?
    (4)dense特征在深度模型中怎么处理的?
    (5)为什么需要Deep部分？FM不work吗？
3.LR推导（都是灵魂拷问，后面查资料补上答案）
    (1)先说思路，关键步骤
    - (1)定义模型输出a=wx+b 
    - (2)将输出值映射到0到1之间yp=sigmoid(a)
    - (3)定义loss=sum(ylogyp + (1-y)log(1-yp))
    - (4)梯度下降，求解w和b
    (2)LR实现（没写,让后面补上发邮箱，也没发。。。）
    (3)LR有最优解吗
    (4)LR能直接求解吗？不用梯度下降
    (5)交叉熵损失是凸函数吗
    - 第(3)、(4)、(5)的答案: LR有最优解，LR不能直接求解，交叉熵损失是凸函数
    - 交叉熵损失是凸函数，逻辑回归对应的优化问题是凸优化问题。对于凸优化问题，所有的极小值都是全局极小值。（凸函数的判断方法：hessian矩阵是半正定的）
    - 经典的优化方法可以分为直接法和迭代法，直接法就是能够直接给出优化问题最优解的方法。直接法需要满足两个条件:(1)L函数是凸函数(2)L对theta求导等于0这个式子有闭式解。经典的例子是考虑L2正则的平方损失。

## 二面
1.算法题：给定一个数组，返回top k大的子集
2.项目介绍：略
3.围绕项目展开的技术问题
    (1)简单介绍lambdaMART原理
    - pair-wise的排序算法，主要包括两个关键点：lambda和梯度提升树；梯度提升树就是根据结果的负梯度来得到下一棵树。lambda就是指梯度，与提升树的梯度略有不同，这里的lambda是考虑了排序指标ndcg
    (2)ndcg的定义
    - label_gain和位置折损
    (3)ndcg的具体计算公式
    - n表示normalized，是指归一化稀疏，ideal的排序情况下的dcg。dcg里的分子是2的x次方-1，分母是log(l+1) l表示样本在一个group里的位置
    (4)lambda梯度具体是什么
    - lambdaMART有lambdarank演进而来，lambda在lambdarank模型里是根据交叉熵损失求导得到的梯度。在LambdaMART里，lambda是在交叉熵损失求导结果的基础上乘以delta_ndcg的绝对值。这样定义lambda的好处在于，不需要直接对排序指标ndcg求导，也能达到优化排序结果的目的
    (5)DCN的原理
    - CTR模型里一般有两种结构：串行、并行。像PNN这种，就是串行结构，把embedding结果作为神经网络的输入。像wide&Deep、DeepFM、DCN都是属于并行结构，由wide和Deep两部分组成。这三种模型在wide部分有差异，Deep部分都是全连接网络。wide&Deep的wide部分是只考虑一阶信息，DeepFM是考虑了二阶交叉，DCN是考虑更高阶的显示交叉
    (6)DCN里的高阶交叉怎么做的
    - 高阶交叉事cross net来实现的。input作为X0，X(l+1) = X0*X(l)*W + X(l) + B
    (7)PNN里用的外积的定义
    - 笛卡尔积，一个k维的隐向量乘以一个k维的隐向量，会得到一个k*k的矩阵。外积是是元素相乘后得到一个矩阵。内积是对应元素相乘后再加和，得到一个元素值
    (8)过拟合怎么处理
    - (1)正则化 (2)模型结构简化:层数降低、神经元数量减少、树的深度降低等
    (9)l1、l2正则的定义,作用是什么
    - l1是对参数求绝对值，l2是对参数求平方。正则化的作用都是为了防止模型过拟合。l1是能得到稀疏解，l2是限制参数的大小趋于0
    (10)drop_out是什么
    - drop_out是指神经元做随机丢弃，能一定程度降低模型过拟合的风险
