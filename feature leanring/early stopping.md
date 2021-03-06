﻿### 1.0 early stopping
#### 1.0 early stopping
![early_stop_1.png](https://i.imgur.com/ju1QlUX.png)

算法7.1：用于确定最佳训练时间量的提前终止元算法。这种元算法是一种通用策略，可以很好地在各种训练算法和各种量化验证集误差的方法上工作。

![early_stopping_1.png](https://i.imgur.com/HClBPis.png)

提前终止需要验证集，意味着某些训练数据不能被馈送到模型。为了更好地利用这一额外的数据，我们可以在完成提前终止的首次训练之后，进行额外的训练。在第二轮，即额外的训练步骤中，所有的训练数据都包括在内。有两个基本的策略都可以用于第二轮训练：

策略1，再次初始化模型，然后使用所有数据再次训练。在这个第二次训练过程中，使用第一轮提前终止训练确定的最佳步数。

此过程有一些细微之处。例如，重新训练时，对参数进行相同次数的更新和对数据集进行相同次数哪一个更好。由于训练数据集变大，第二次训练中，每一次遍历数据集将会更多的更新参数。

![early_stopping_2.png](https://i.imgur.com/IpZUwBe.png)

策略2，保持从第一轮训练获得的参数，然后使用全部的数据继续训练。在这个阶段，已经没有验证集指导我们需要在训练多少步后终止。取而代之，我们可以监控验证集的平均损失函数，并继续训练，直到它低于提前终止过程终止时的目标值。此策略避免了重新训练模型的高成本，但表现并没有那么好。

![early_stopping_3.png](https://i.imgur.com/0YkXGUM.png)

提前终止对减少训练过程的计算成本也是有用的。除了由于限制训练的迭代次数而明显减少的计算成本，还带来了正则化的益处。

#### 1.2 提前终止为何具有正则化效果
![early_stopping_reg_1.png](https://i.imgur.com/dvCQFdP.png)

上图为提前终止效果的示意图。(左)实线轮廓线表示负对数似然的轮廓。虚线表示从原点开始的 SGD 所经过的轨迹。 提前终止的轨迹在较早的点 w~ 处停止，而不是停止在最小化代价的点 w<sup>∗</sup>处。(右)为了对比，使用 L2正则化效果的示意图。虚线圆圈表示L2惩罚的轮廓，L2惩罚使得总代价的最小值比非正则化代价的最小值更靠近原点。

### 2.0 early stopping优缺点

缺点：不能独立的处理梯度下降和防止过拟合，因为过早的停止梯度下降也就是停止了优化代价函数，导致模型考虑的的东西变得复杂。

优点：运行一次梯度下降，可以找到权重`w`较小值、中间值、较大值等；`L2`正则化则需要结合网络寻找最优`λ`

参考：

[深度学习 7.8 提前终止](https://github.com/exacity/deeplearningbook-chinese)
