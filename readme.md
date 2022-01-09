## Reducing bias in AI-Based financial Services



## 一、Job Requirements 

​	从某一方面（隐私、公平、对抗攻击、可解释等）入手，自行实现一个简单的可信机器学习算法（如基于SVM、决策树、神经网路、概率图模型等），课堂展示并简要介绍算法原理.



## 二、Job Background

​		人工智能（AI）为改变我们分配信贷和处理风险的方式提供了一个机会，并创造了更公平、更包容的系统。人工智能可以避免传统的信用报告和评分系统，这有助于抛弃现有的偏见，使它成为一个难得的，改变现状的机会。然而，人工智能很容易朝另一个方向发展，加剧现有的偏见，创造出一个循环，加强有偏见的信贷分配，同时使贷款歧视更难找到。我们将通过开源模型Fairlearn来释放积极的一面，缓解偏见消极的一面。



## 三、The experiment content

- **涉及领域：**
  - 金融贷款方面的决策分析。我们分析的数据是原始数据经过人工简单处理过的，是为了展现准确性方面的悬殊差异。
- **机器学习任务：**
  - 二元性分类
- **机器学习公平任务：**
  - 使用Fairlearn metrics和Fairlearn dashboard来评估模型的公平。
  - 使用Fairlearn中的改进算法来改进模型的公平水平。
- **性能指标：**
  - ROC曲线下的面积。
  - 平衡过后的准确率。
- **公平指标：**
  - Equalized-odds difference.
- **改进的算法:**
  - `fairlearn.reductions.GridSearch`

  - `fairlearn.postprocessing.ThresholdOptimizer`
  
    

​	本次项目中我们模拟了贷款决策中出现的性别准确性差异问题。具体来说，我们考虑的情况是，算法工具根据历史数据进行训练，其对贷款申请人的预测被用于对申请人做出决定。（关于涉及信用额度决策中基于性别的歧视的例子，请看[这里](https://www.nytimes.com/2019/11/10/business/Apple-credit-card-investigation.html)。）

​		我们使用[UCI信用卡数据集](https://archive.ics.uci.edu/ml/datasets/default+of+credit+card+clients)。为了这个练习，我们修改了原始的UCI数据集：我们引入了一个合成特征，该特征对女性客户有很强的预测能力，但对男性申请人没有信息。我们拟合了各种预测客户违约的模型。我们表明，一个没有公平意识的训练算法可以导致一个预测器对女性的准确率远远高于男性，而且简单地从训练中删除敏感特征（在这种情况下为性别）是不够的。最后，我们使用fairlearn中的两个缓解算法`ThresholdOptimizer`和`GridSearch`来改进模型。