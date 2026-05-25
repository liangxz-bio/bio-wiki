---
source_url: https://mp.weixin.qq.com/s/（微信公众号·深夜努力写Python）
ingested: 2026-05-21
sha256: c254964ae3a933c7e409102e4c17b82dd49385e8e1e29af19f1d359abd158fe6
citation: "cos大壮. 最强组合！随机森林+贝叶斯优化：超参数自动调优. 微信公众号·深夜努力写Python, 2026."
---

# 最强组合！随机森林+贝叶斯优化 ！！

> **来源：** 深夜努力写Python

---

哈喽，我是cos大壮！~

今儿和大家聊一个话题，利用随机森林+贝叶斯优化，超参数自动调优：高效搜索最优参数组合，提升模型性能。

在聊这个话题之前，首先简单说一说随机森林与贝叶斯优化结合后的优势~

首先，**随机森林**是一种集成学习方法，通过构建多棵决策树，并通过投票机制决定最终的分类或回归结果。它在处理高维度数据时表现出色，能够有效防止过拟合，并能处理缺失数据。由于其非参数的性质，随机森林对数据的分布没有严格要求，适用于许多类型的任务。

而**贝叶斯优化**是一种基于贝叶斯理论的全局优化方法，通常用于优化复杂的、评估代价较高的函数。贝叶斯优化通过构建代理模型（如高斯过程）来估计目标函数，然后通过计算采集函数来选择下一个评估点，从而找到函数的极值。

当将**随机森林**和**贝叶斯优化**结合时，通常是将随机森林作为代理模型，替代高斯过程等传统模型。

**内容非常完整，大家看可以先收藏，慢慢学习~

**

这样做的优势包括：

- **提高计算效率**：随机森林可以非常快速地进行预测，并且不需要计算复杂的协方差矩阵，因此在多次迭代的贝叶斯优化中具有较高的计算效率。
- **避免过拟合**：贝叶斯优化中的代理模型若选用高斯过程，在数据量非常大时可能出现计算上的瓶颈，而随机森林由于其对噪声的容忍能力，能够有效避免过拟合。
- **非线性建模能力**：与高斯过程相比，随机森林能够更好地处理高维度和复杂的非线性关系，这对于许多实际的优化问题尤为重要。
- **对不确定性处理能力**：随机森林的结构使其能够较为稳健地处理数据中的噪声与不确定性，进而提高贝叶斯优化过程中的决策质量。

## 深究原理

### 随机森林
随机森林的核心思想是通过训练多个决策树并将它们的结果结合起来，从而提高模型的泛化能力。

每棵树的训练过程都依赖于**Bootstrap抽样**和**特征子集选择**。

具体步骤：

- **Bootstrap抽样**：从训练数据中随机抽取多个样本，允许重复抽取。
- **决策树的训练**：对于每一棵树，在每个节点分裂时，不是考虑所有特征，而是随机选择一个特征子集来寻找最佳分裂点。
随机森林的预测是所有树的结果的加权平均（回归）或者多数投票（分类）。

其误差通常由两部分组成：

- **偏差**：模型的预测值与真实值之间的系统性差异。
- **方差**：模型预测值的波动性。
随机森林通过构建大量的树并取平均，能够有效减少方差，从而提高模型的稳定性。

这部分内容在之前的推文中聊过，在我的知识星球的《200个核心算法模型》中也有非常详细的阐述。

### 贝叶斯优化
贝叶斯优化的目标是最小化或最大化一个黑箱函数 ，其中  是优化变量，且该函数的评估代价较高。

贝叶斯优化通过使用代理模型来估计目标函数，然后根据代理模型的信息选择最有可能产生最优解的点。

贝叶斯优化的核心流程包括：

- **选择代理模型**：通常使用高斯过程（GP）来作为代理模型，GP通过训练数据构建一个概率分布来估计目标函数。
- **选择采集函数**：采集函数（Acquisition Function）用于衡量每个候选点的潜在价值，常见的采集函数有期望改进（EI）、概率改进（PI）等。
- **选择下一个评估点**：通过优化采集函数来选择下一个评估点。
贝叶斯优化的关键是利用代理模型来平衡探索（exploration）和开发（exploitation）。

探索是指尝试不确定性较大的区域，而开发是指在已知的高值区域进一步优化。

### 随机森林+贝叶斯优化 -> 内在逻辑
在传统贝叶斯优化中，常用高斯过程作为代理模型。

但当目标函数较为复杂或数据量较大时，高斯过程的计算可能会变得非常昂贵且不稳定。

此时，**随机森林**可以作为一个更有效的代理模型。

随机森林作为代理模型有以下几个特点：

- 随机森林可以通过多棵决策树来对目标函数进行建模，避免了高斯过程在处理高维度数据时可能面临的困难。
- 随机森林能够较好地处理非线性函数，因此可以用来逼近更复杂的目标函数。
- 随机森林的训练和预测都相对高效，能够减少贝叶斯优化中代理模型训练的计算开销。

## 深究细节
在贝叶斯优化中，如果使用随机森林作为代理模型，那么贝叶斯优化的步骤可以表述为：

**1. 训练随机森林模型**：

假设目标函数是 ，我们使用训练数据集  来训练随机森林模型。

随机森林的预测是所有树的平均预测：

其中  是第  棵树的预测结果， 是树的数量。

**2. 计算采集函数**：

采集函数  衡量了每个候选点  的潜在价值。常用的采集函数如期望改进（EI）可以表示为：

其中  是当前已知的最佳目标值。

**3. 选择下一个评估点**：

根据采集函数，选择下一个评估点 ：

**4. 更新训练数据**：

通过评估  来更新训练数据集 ，然后重新训练随机森林模型。

**5. 迭代**：

重复以上步骤，直到满足停止条件（如最大迭代次数，或目标函数达到满意值）。

总之，将随机森林与贝叶斯优化结合，能够有效提高贝叶斯优化的效率和精度。随机森林作为代理模型，在处理高维、复杂的目标函数时表现出色，避免了高斯过程在大数据集上的计算瓶颈，同时通过集成的方式增强了模型的稳定性。

这就会使得随机森林和贝叶斯优化的结合在许多实际问题中成为一种强大的优化工具。

## 完整案例
这里咱们基于一个回归问题，使用**随机森林**回归器作为目标模型，通过贝叶斯优化来自动调节其超参数，以提升模型的预测性能。

通过虚拟数据集来进行实验，并通过图形展示贝叶斯优化过程中的超参数调优效果。

我们创建一个虚拟的数据集，该数据集具有单个输入特征与目标值之间的非线性关系。

然后，我们使用**随机森林回归器**训练一个回归模型，目标是通过贝叶斯优化自动调节超参数，使得模型的预测性能达到最优。

#### 1. 数据生成
import numpy as np**import matplotlib.pyplot as plt****# 设置随机种子**np.random.seed(42)****# 生成虚拟数据集**X = np.linspace(0, 10, 1000).reshape(-1, 1)  # 1000个数据点**y = np.sin(X) + 0.1 * np.random.randn(1000, 1)  # 添加噪声的sin函数****# 绘制数据图**plt.figure(figsize=(8, 6))**plt.scatter(X, y, color='blue', label='Data points', alpha=0.7)**plt.title('Generated Data for Regression')**plt.xlabel('Feature (X)')**plt.ylabel('Target (y)')**plt.legend()**plt.grid(True)**plt.show()**代码中生成了一个包含1000个数据点的简单回归数据集，目标值  并添加了噪声。

我们使用这个数据集来训练回归模型。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/kibwfTuPM4lic4ibSxcHpdibcTsVNgrWehcbIGXwL4Rb4YN5Ffb1qIUugghJ7oj7krrt4aGGkFiaaicgmZPA1f6RpiaEQ/640?wx_fmt=png&from=appmsg)

#### 2. 随机森林回归模型
接下来，我们使用**随机森林回归器**来训练模型：

from sklearn.ensemble import RandomForestRegressor**from sklearn.model_selection import train_test_split**from sklearn.metrics import mean_squared_error****# 划分训练集和测试集**X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)****# 初始化随机森林回归器**rf_model = RandomForestRegressor(n_estimators=100, random_state=42)****# 训练模型**rf_model.fit(X_train, y_train)****# 预测**y_pred = rf_model.predict(X_test)****# 计算均方误差**mse = mean_squared_error(y_test, y_pred)**print(f'Mean Squared Error on Test Set: {mse:.4f}')**使用80%的数据作为训练集，20%的数据作为测试集。然后，我们训练一个随机森林回归模型并计算测试集的均方误差（MSE），这将作为模型性能的衡量标准。

### 贝叶斯优化调参
在实际应用中，随机森林模型有多个超参数需要调优，例如**n_estimators**（树的数量）和**max_depth**（树的最大深度）。

我们使用**贝叶斯优化**来自动寻找最佳的超参数组合，从而提高模型性能。

#### 1. 贝叶斯优化库
我们使用**BayesianOptimization**库来进行贝叶斯优化，定义优化目标函数，并搜索最优超参数。

from bayes_opt import BayesianOptimization****# 定义目标函数（即优化函数）**def rf_cv(n_estimators, max_depth):**    # 将超参数转换为整数**    n_estimators = int(n_estimators)**    max_depth = int(max_depth)****    # 使用给定的超参数训练模型**    model = RandomForestRegressor(n_estimators=n_estimators, max_depth=max_depth, random_state=42)**    model.fit(X_train, y_train)**    **    # 预测并计算均方误差**    y_pred = model.predict(X_test)**    mse = mean_squared_error(y_test, y_pred)**    **    # 返回负均方误差，因为贝叶斯优化最小化目标函数**    return -mse****# 设置贝叶斯优化的超参数搜索范围**pbounds = {'n_estimators': (10, 200), 'max_depth': (1, 20)}****# 初始化贝叶斯优化器**optimizer = BayesianOptimization(f=rf_cv, pbounds=pbounds, random_state=42)****# 开始优化**optimizer.maximize(init_points=5, n_iter=25)****# 输出最优参数组合**print("Best parameters found:", optimizer.max['params'])**代码中定义了一个目标函数 rf_cv，该函数根据给定的超参数训练一个随机森林模型，并返回其均方误差的负值（贝叶斯优化默认最小化目标函数）。

我们设置了**n_estimators**和**max_depth**的搜索范围，并通过贝叶斯优化寻找最优的超参数。

### 优化过程的可视化
在优化过程中，我们可以通过图形化来展示模型在不同超参数组合下的表现。

- **训练数据与预测曲线**：展示训练数据与模型预测的对比。
- **超参数优化过程中的均方误差**：展示每次迭代后贝叶斯优化选择的超参数和对应的模型性能。
- **超参数空间的探索情况**：展示贝叶斯优化探索的超参数空间。
- **优化后的预测结果**：展示优化后的预测效果。

#### 1. 训练数据与预测曲线
# 绘制训练数据与预测曲线**X_range = np.linspace(0, 10, 1000).reshape(-1, 1)**y_pred_range = rf_model.predict(X_range)****plt.figure(figsize=(10, 8))**plt.scatter(X, y, color='blue', label='Data points', alpha=0.6)**plt.plot(X_range, y_pred_range, color='red', label='Random Forest Prediction', linewidth=2)**plt.title('Random Forest Regression Prediction vs Data')**plt.xlabel('Feature (X)')**plt.ylabel('Target (y)')**plt.legend()**plt.grid(True)**plt.show()**

![](https://mmbiz.qpic.cn/sz_mmbiz_png/kibwfTuPM4lic4ibSxcHpdibcTsVNgrWehcbZ5eAfD9iahibMUl6gceGoZR6dN6kDKRfcxlzpzB8CfG0wNLgvkXd8lEQ/640?wx_fmt=png&from=appmsg)

#### 2. 贝叶斯优化过程中的均方误差
# 获取贝叶斯优化的结果并绘制均方误差曲线**iterations = np.arange(len(optimizer.res))**mse_values = [-res['target'] for res in optimizer.res]****plt.figure(figsize=(10, 8))**plt.plot(iterations, mse_values, marker='o', color='green', label='MSE per iteration')**plt.title('Bayesian Optimization - MSE Across Iterations')**plt.xlabel('Iteration')**plt.ylabel('Mean Squared Error (MSE)')**plt.legend()**plt.grid(True)**plt.show()**

![](https://mmbiz.qpic.cn/sz_mmbiz_png/kibwfTuPM4lic4ibSxcHpdibcTsVNgrWehcb8oia9aTwq4biaOZIib0Lv5A6Wrx5usHLB7rKKJGVhEzicjzGxj6kNyRo4Q/640?wx_fmt=png&from=appmsg)

#### 3. 超参数空间的探索情况
# 绘制超参数空间的探索情况**n_estimators_values = [res['params']['n_estimators'] for res in optimizer.res]**max_depth_values = [res['params']['max_depth'] for res in optimizer.res]****plt.figure(figsize=(10, 8))**plt.scatter(n_estimators_values, max_depth_values, color='purple', label='Explored Hyperparameters', alpha=0.7)**plt.title('Hyperparameter Space Exploration during Bayesian Optimization')**plt.xlabel('Number of Estimators (n_estimators)')**plt.ylabel('Max Depth (max_depth)')**plt.legend()**plt.grid(True)**plt.show()**

![](https://mmbiz.qpic.cn/sz_mmbiz_png/kibwfTuPM4lic4ibSxcHpdibcTsVNgrWehcbaYXVk9IticZjDiaibgrwiaNLpp9yOEmdwhQia09h118sIibazTmGB6bHsviag/640?wx_fmt=png&from=appmsg)

#### 4. 优化后的预测结果
# 使用最优超参数训练最终模型并绘制预测结果**best_params = optimizer.max['params']**best_model = RandomForestRegressor(n_estimators=int(best_params['n_estimators']),**                                   max_depth=int(best_params['max_depth']), random_state=42)**best_model.fit(X_train, y_train)**y_pred_best = best_model.predict(X_range)****plt.figure(figsize=(10, 8))**plt.scatter(X, y, color='blue', label='Data points', alpha=0.6)**plt.plot(X_range, y_pred_best, color='orange', label='Optimized Model Prediction', linewidth=2)**plt.title('Optimized Random Forest Regression Prediction')**plt.xlabel('Feature (X)')**plt.ylabel('Target (y)')**plt.legend()**plt.grid(True)**plt.show()**

![](https://mmbiz.qpic.cn/sz_mmbiz_png/kibwfTuPM4lic4ibSxcHpdibcTsVNgrWehcbUYQa4DxwKhSkUMykFIZMOFvu2fYiczrjHsWxvm7gDiceygsVLiaCW3P6A/640?wx_fmt=png&from=appmsg)

### 算法优化点和调参流程
**优化点**：

- **更多的超参数调节**：除了 n_estimators 和 max_depth，还可以进一步调节 min_samples_split、min_samples_leaf、max_features 等超参数。
- **使用更复杂的代理模型**：尽管随机森林是一个很好的选择，但在某些任务中，高斯过程等其他代理模型可能会更加精确。
**调参流程**：

- **定义问题和数据集**：明确优化问题及其目标函数（例如回归、分类问题等），并准备相应的数据集。
- **选择超参数范围**：根据实际问题，选择待优化的超参数及其搜索范围。
- **贝叶斯优化配置**：初始化贝叶斯优化器并设置优化目标函数、超参数空间。
- **迭代优化**：运行贝叶斯优化，监控每次迭代的结果，并根据最优参数进行模型训练。
- **评估与可视化**：通过交叉验证、MSE等评价指标评估模型性能，并通过图形展示优化过程。
整个内容，我们通过使用贝叶斯优化对随机森林回归模型的超参数进行自动调优，我们能够高效地找到最优参数组合，显著提高模型的性能。通过图形化展示优化过程和模型的预测效果，我们可以直观地理解贝叶斯优化如何在每次迭代中提高模型预测的准确性。**

## 往期精华
[知识星球，硬核学习圈子](https://mp.weixin.qq.com/s?__biz=Mzk0MjUxMzg3OQ==&mid=2247496237&idx=1&sn=e6d1db22bab530ae7dd60ad5e0be6793&scene=21#wechat_redirect)

[讲透200个机器学习模型](https://mp.weixin.qq.com/s?__biz=Mzk0MjUxMzg3OQ==&mid=2247497552&idx=1&sn=fa466d927f47e0173c13802ff739deaf&scene=21#wechat_redirect)

[十大统计检验方法](https://mp.weixin.qq.com/s?__biz=Mzk0MjUxMzg3OQ==&mid=2247498010&idx=1&sn=59646bdafcfdd4d2e7ab00da7137aae1&scene=21&token=423242633&lang=zh_CN#wechat_redirect)**

[【优化算法】网格搜索](https://mp.weixin.qq.com/s?__biz=Mzk0MjUxMzg3OQ==&mid=2247498510&idx=1&sn=0d63322ef3fa884e5d89d10e507ecbcb&scene=21#wechat_redirect)

[最强组合，随机森林和XGBoost](https://mp.weixin.qq.com/s?__biz=Mzk0MjUxMzg3OQ==&mid=2247498482&idx=1&sn=c15c802a2c58de1795e72b90445c1ccc&scene=21#wechat_redirect)

[融合ARIMA+LSTM+Prophet融合的时序预测](https://mp.weixin.qq.com/s?__biz=Mzk0MjUxMzg3OQ==&mid=2247497518&idx=1&sn=0f52e16313d4cf7b8d95c2b39125a63e&scene=21#wechat_redirect)

[融合SVR+XGBoost+LSTM，非平稳时序预测](https://mp.weixin.qq.com/s?__biz=Mzk0MjUxMzg3OQ==&mid=2247497808&idx=1&sn=6e2e1f09e04e0aed42ece2a69c8ae47b&scene=21#wechat_redirect)

[靠三四区论文，进大厂](https://mp.weixin.qq.com/s?__biz=Mzk0MjUxMzg3OQ==&mid=2247498041&idx=1&sn=54795426eb94703f9086d1ddd73377fc&scene=21#wechat_redirect)

[开年发论文！登Nature正刊](https://mp.weixin.qq.com/s?__biz=Mzk0MjUxMzg3OQ==&mid=2247497935&idx=1&sn=1b4b146df4357a12f0b1b647fde4d584&scene=21#wechat_redirect)

## **有任何问题评论区留言~

我是cos大壮，只写干货，最后感谢大家的点赞或者转发~

**
#cos大壮的机器学习#cos大壮的机器学习实战
