### 背景
在很多情况下数据中的一小段携带了数据集中大部分的信息，其他的信息要不就是噪声，要不就是毫不相干。

### 1.0 矩阵A奇异值分解
直观上，奇异值分解将矩阵分解为若干个秩一矩阵之和，用公式表示：

![矩阵A奇异值分解](https://www.zhihu.com/equation?tex=%281%29+%5Cquad%5Cquad+%5Cqquad+A+%3D+%5Csigma_1+u_1v_1%5E%7B%5Crm+T%7D%2B%5Csigma_2+u_2v_2%5E%7B%5Crm+T%7D%2B...%2B%5Csigma_r+u_rv_r%5E%7B%5Crm+T%7D)

其中等式右边每一项前的系数σ就是奇异值,μ和υ分别表示列向量，秩一矩阵的意思是矩阵秩为1，注意到每一项uv<sup>T</sup>都是秩为1的矩阵。假定奇异值满足δ<sub>1</sub> >= δ<sub>2</sub> >= ... >= δ<sub>r</sub> > 0。

### 1.1 奇异值的物理意义
奇异值往往对应着矩阵中隐含的重要信息，且重要性和奇异值大小正相关。每个矩阵A都可以表示为一系列秩为1的小矩阵之和，而奇异值则衡量了这些小矩阵对于A的权重。

### 1.2 奇异值的应用
在图像处理领域，奇异值不仅可以应用在**数据压缩**上，还可以对**图像去噪**。如果一副图像包含噪声，我们有理由相信那些较小的奇异值就是由于噪声引起的。当我们强行令这些较小的奇异值为0时，就可以去除图片中的噪声。

### 1.3 矩阵M的奇异值分解
A<sub>m*n</sub> = U<sub>mxm</sub> * Σ<sub>mxn</sub> * V<sup>T</sup><sub>nxn</sub>

U<sub>mxm</sub>:AxA<sup>T</sup>的正交的特征向量；

Σ<sub>mxn</sub>:从大到小排列的奇异值；

V<sup>T</sup><sub>nxn</sub>:A<sup>T</sup>xA的正交的特征向量；


当矩阵M作用在正交单位向量ν1和v2上之后，得到Mv1和Mv2也是正交的。令u1和u2分别是Mv1和Mv2方向上的单位向量，即：

Mv1 = σ1u1

Mv2 = σ2u2

写在一起就是M[v1 v2]=[σ1u1 σ2u2]，整理得：

![svd_2.png](https://i.imgur.com/cdXpiLB.png)

这样就得到矩阵M的的奇异值分解。奇异值σ1、σ2分别是Mv1和Mv2的长度。很容易可以把结论推广到一般n维情形。

奇异值分解把线性变换清晰地分解为**旋转**、**缩放**、**投影**这三种基本线性变换。

![奇异值分解_旋转_缩放_投影](https://pic4.zhimg.com/v2-ea67bee7f332fa7bab9bb4ccf19f17e4_r.jpg)

图形解释

![svd_14.png](https://i.imgur.com/ZciP6aT.png)

### 1.4 奇异值分解的几何意义
对于任何的一个矩阵，找到一组两两正交单位向量序列，使得矩阵作用在此向量序列上后得到新的向量序列保持两两正交。

![svd_13.png](https://i.imgur.com/2FXPCpI.png)

### 1.5 奇异值的几何意义
这组变换后的新的向量序列的长度。


![svd_12.png](https://i.imgur.com/KOX11WO.jpg)

矩阵将二维平面中单位圆变化成椭圆，那么奇异值正好是椭圆两个半轴长。

推广到一般情形：一般矩阵A将单位球||x||<sub>2</sub>=1变换为超球面，那么矩阵的每个奇异值就是超椭圆的每条半轴的长度。

### 2.1 SVD 是矩阵分解的一种类型。
A<sub>m*n</sub> = U<sub>mxm</sub> * Σ<sub>mxn</sub> * V<sup>T</sup><sub>nxn</sub>

![svd_1.png](https://i.imgur.com/wDz4Wqu.png)

矩阵U和V都定义为正交矩阵，而矩阵Σ定义为对角矩阵。注意，矩阵Σ不一定是方阵。

对角矩阵Σ对角线上的元素被称为矩阵A的奇异值。事实上，A的左奇异值U是AA<sup>T</sup>的特征向量。A的右奇异值V是A<sup>T</sup>A的特征向量。A的非零奇异值是A<sup>T</sup>A特征值的平方根，同时也是AA<sup>T</sup>特征值的平方根。

### 2.2 SVD特点
优点：简化数据，去除噪声，优化算法的结果

缺点：数据的转换可能难以理解

使用的数据类型：数值型数据

### 3.0 基于物品相似度的推荐引擎原理
```python
u1未评分物品item
循环u1中评分的列
    假设u1对f1评分userRating，item未评分；u2对f1评分，item评分；
    步骤一：计算u2评分中，f1,item的相似度similarity；
    步骤二：计算总的相似度simTotal, 对应评分值userRating*similarity求和ratSimTotal
计算估计评分：ratSimTotal/simTotal

步骤三：对预估的评分物品按照得分排序itemScores
```

### 3.1 基于物品相似度的推荐引擎 代码实现
```python
# 基于物品相似度的推荐引擎
def standEst(dataMat, user, simMeas, item):
    """standEst(计算某用户未评分物品中，以对该物品和其他物品评分的用户的物品相似度，然后进行综合评分)

    Args:
        dataMat         训练数据集
        user            用户编号
        simMeas         相似度计算方法
        item            未评分的物品编号
    Returns:
        ratSimTotal/simTotal     评分（0～5之间的值）
    """
    # 得到数据集中的物品数目
    n = shape(dataMat)[1]
    # 初始化两个评分值
    simTotal = 0.0
    ratSimTotal = 0.0
    # 遍历行中的每个物品（对用户评过分的物品进行遍历，并将它与其他物品进行比较）
    for j in range(n):
        userRating = dataMat[user, j]
        # 如果某个物品的评分值为0，则跳过这个物品
        if userRating == 0:
            continue
        # 寻找两个用户都评级的物品
        # 变量 overLap 给出的是两个物品当中已经被评分的那个元素的索引ID
        # logical_and 计算x1和x2元素的真值。
        overLap = nonzero(logical_and(dataMat[:, item].A > 0, dataMat[:, j].A > 0))[0]
        # 如果相似度为0，则两着没有任何重合元素，终止本次循环
        if len(overLap) == 0:
            similarity = 0
        # 如果存在重合的物品，则基于这些重合物重新计算相似度。
        else:
            similarity = simMeas(dataMat[overLap, item], dataMat[overLap, j])
        # print 'the %d and %d similarity is : %f'(iten,j,similarity)
        # 相似度会不断累加，每次计算时还考虑相似度和当前用户评分的乘积
        # similarity  用户相似度，   userRating 用户评分
        simTotal += similarity
        ratSimTotal += similarity * userRating
    if simTotal == 0:
        return 0
    # 通过除以所有的评分总和，对上述相似度评分的乘积进行归一化，使得最后评分在0~5之间，这些评分用来对预测值进行排序
    else:
        return ratSimTotal/simTotal
```

### 3.2 基于SVD的评分估计
```python
# 基于SVD的评分估计
# 在recommend() 中，这个函数用于替换对standEst()的调用，该函数对给定用户给定物品构建了一个评分估计值
# user 未对 item评分
def svdEst(dataMat, user, simMeas, item):
    """svdEst( )

    Args:
        dataMat         训练数据集
        user            用户编号
        simMeas         相似度计算方法
        item            未评分的物品编号
    Returns:
        ratSimTotal/simTotal     评分（0～5之间的值）
    """
    # 物品数目
    n = shape(dataMat)[1]
    # 对数据集进行SVD分解
    simTotal = 0.0
    ratSimTotal = 0.0
    # 奇异值分解
    # 在SVD分解之后，我们只利用包含了90%能量值的奇异值，这些奇异值会以NumPy数组的形式得以保存
    U, Sigma, VT = la.svd(dataMat)

    # # 分析 Sigma 的长度取值
    analyse_data(Sigma, 20)

    # 如果要进行矩阵运算，就必须要用这些奇异值构建出一个对角矩阵
    Sig4 = mat(eye(4) * Sigma[: 4])

    # 利用U矩阵将物品转换到低维空间中，构建转换后的物品(物品+4个主要的特征)
    xformedItems = dataMat.T * U[:, :4] * Sig4.I

    # 对于给定的用户，for循环在用户对应行的元素上进行遍历
    # 这和standEst()函数中的for循环的目的一样，只不过这里的相似度计算时在低维空间下进行的。
    for j in range(n):
        userRating = dataMat[user, j]
        if userRating == 0 or j == item:
            continue
        # 相似度的计算方法也会作为一个参数传递给该函数
        similarity = simMeas(xformedItems[item, :].T, xformedItems[j, :].T)
        # for 循环中加入了一条print语句，以便了解相似度计算的进展情况。如果觉得累赘，可以去掉
        print 'the %d and %d similarity is: %f' % (item, j, similarity)
        # 对相似度不断累加求和
        simTotal += similarity
        # 对相似度及对应评分值的乘积求和
        ratSimTotal += similarity * userRating
    if simTotal == 0:
        return 0
    else:
        # 计算估计评分
        return ratSimTotal/simTotal
```

svd选出最重要的4个特征，步骤一、二、三和基于物品相似度的推荐引擎原理一致。

此处svd的应用仍然是降维。

### 3.3 基于SVD的推荐系统
假设我们现在有评分矩阵 V∈R<sup>n×m</sup>，SVD实际上就是去找到两个矩阵： U∈R<sup>f×n</sup>，M∈R<sup>f×m</sup>，其中矩阵U表示User和feature之间的联系，矩阵V表示Item和feature之间的联系。

V=U<sup>T</sup>M

![svd_2.png](https://i.imgur.com/LKgxaes.png)

### 4.0 机器学习中SVD和PCA一直没有搞的特别清楚，应该如何理解呢？

PCA降维的大致思想就是： 挑选特征明显的、显得比较重要的信息保留下来。那么关键就是【特征明显的，重要的信息】：
1. 同一个维度内的数据，方差大的比较明显，因为方差大表示自己和平均水平差异大，有个性，降维后也最可能分的开～
2. 两个不同维度间关联度越小越好，因为关联度小表示这两个维度表征共同信息的量比较少，最理想就是两个维度不相关，相关度为0（相关度可以用协方差cov(a,b)表示），在线性空间内表现为两个维度正交

协方差矩阵的主对角线和其余元素正好可以分别表示方差和协方差，而根据两条标准又很容易想到求特征值和特征向量;

下面说一下为啥还有SVD，上面我们说PCA针对的是协方差矩阵C，但你得知道协方差矩阵是个方阵啊，难道不是方阵我们就不资瓷么？？ 所以就有了SVD～～大概可以把SVD看作是对非方阵做PCA处理的一种方式啦，毕竟两者的套路都差不多，分解出特征值（SVD里是奇异值，数据XX‘的特征值的平方根），挑比较大的特征值对应的特征向量构成投影矩阵，然后做线性变换（将数据X投影到低维空间）

参考：

[奇异值的物理意义是什么？](https://www.zhihu.com/question/22237507 "奇异值的物理意义是什么？")

[机器学习中SVD和PCA一直没有搞的特别清楚，应该如何理解呢？](https://www.zhihu.com/question/38319536/answer/131150925)

[漫谈奇异值分解](http://charleshm.github.io/2016/03/Singularly-Valuable-Decomposition/)

[奇异值分解(SVD) --- 几何意义](http://blog.sciencenet.cn/blog-696950-699432.html)

[SVD在推荐系统中的应用](http://charleshm.github.io/2016/03/SVD-Recommendation-System/#)