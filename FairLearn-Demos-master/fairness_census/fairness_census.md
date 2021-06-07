# Teaching your models to play fair

​		在这个笔记本中，你将使用fairlearn和公平仪表板来生成人口普查数据集的预测器。这个数据集是一个分类问题--给定一个关于32,000人的数据范围，预测他们的年收入是高于还是低于50,000美元/年。
​		为了本笔记本的目的，我们将把它当作一个贷款决策问题。我们将假装标签表示每个人在过去是否偿还了贷款。我们将使用这些数据来训练一个预测器，以预测以前未见过的个人是否会偿还贷款。我们的假设是，模型预测被用来决定一个人是否应该被提供贷款。
​		我们将首先训练一个没有公平意识的预测器，并表明在一个特定的公平概念下，它导致了不公平的决定，这个概念称为人口均等。然后，我们通过应用fairlearn软件包中的GridSearch算法来缓解不公平。



#### 1.1 准备

#### 1.2 加载数据

#### 1.3 执行数据转换

​		我们将把每个人的性别作为一个受保护的属性（0表示女性，1表示男性），在这种特殊情况下，我们将把这个属性分离出来，从主数据中删除。然后我们执行一些标准的数据预处理步骤，将数据转换成适合ML算法的格式。

#### 1.4 训练无公平意识的预测模型

为了显示fairlearn的效果，我们将首先训练一个不包含公平性的标准ML预测器 为了演示速度，我们使用sklearn的简单逻辑回归估计器。

#### 1.5 评估公平性

我们可以把这个预测器加载到公平性仪表板中，并检查它是如何不公平的（由于我们还没有与AzureML集成，所以有一个关于AzureML的警告）。

#### 1.6 使用GridSearch的缓解措施

​		fairlearn中的GridSearch类实现了Agarwal等人2018年的指数化梯度还原的简化版本。用户提供了一个标准的ML估计器，它被视为一个黑盒子。GridSearch通过生成一连串的重命名和重加权来工作，并为每个人训练一个预测器。
​		在这个例子中，我们指定人口奇偶性（在性别这一受保护的属性上）作为公平性指标。人口均等要求向个人提供机会（在本例中被批准贷款），与受保护阶层的成员身份无关（即女性和男性应该以相同的比例获得贷款）。为了简单起见，我们使用这个指标；一般来说，适当的公平指标并不明显。

​		我们现在就可以把这些预测因子加载到公平性仪表盘中。但是，由于它们的数量，图表会有些混乱。在这种情况下，我们要把那些在误差-差异空间中被其他预测因子支配的预测因子从扫描中移除（注意，差异将只针对受保护的属性进行计算；其他可能受保护的属性将不会被减轻）。一般来说，人们可能不想这样做，因为除了严格优化（给定的受保护属性的）误差和差异外，可能还有其他考虑。
最后，我们可以把主导模型和未减轻的模型一起放入公平性仪表板。
我们看到一个帕累托阵线的形成--代表准确度和差异i预测之间的最佳权衡的预测器集合。在理想的情况下，我们会有一个位于(1,0)的预测器--完全准确，在人口均等的情况下没有任何不公平之处（就受保护的属性 "性别 "而言）。帕累托前线代表了基于我们的数据和选择的估计器，我们可以最接近这个理想。请注意轴的范围--差距轴涵盖的数值比准确度多，所以我们可以在准确度损失不大的情况下大幅减少差距。

​		通过点击图上的各个模型，我们可以更详细地检查它们的差距和准确性指标。在一个真实的例子中，我们将挑选出代表准确度和差距之间最佳权衡的模型，考虑到相关的业务限制。