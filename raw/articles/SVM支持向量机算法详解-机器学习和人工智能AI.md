---
source_url: 微信公众号·机器学习和人工智能AI
ingested: 2026-05-20
sha256: 16ba8ea76f9ddb468e1feee016c1644b3c0b033392a95fa76f2b0b0f7c6d7afd
citation: "机器学习和人工智能AI. 一个强大的算法模型，SVM！！(微信公众号, 2026)"
domain: machine-learning
---

# 一个强大的算法模型，SVM ！！

> **来源：** 机器学习和人工智能AI

**大家好，今儿来聊聊 ****支持向量机****~**

支持向量机（SVM）是一种广泛用于分类和回归分析的强大监督式学习算法。

今天聊的是一个关于 SVM 的实际案例，包括 SVM 的基本概念、模型训练、Python 代码实现及图形展示。

### 1. SVM 概念和公式详解
SVM 的基本理念是找到一个最优超平面（在二维空间中就是一条线），以最大化不同类别之间的边界。核心思想可以总结为以下几点：

- **最大边界**: SVM 试图使得最近的点到决策边界的距离最大化，这些点称为支持向量。
- **超平面**: 在二维空间中是一条线，在高维空间中是一个平面或超平面。
- **核技巧**: 允许 SVM 在更高维度中有效地进行分类，适用于非线性可分的数据。

公式上，SVM 主要解决以下优化问题：

其中， 是超平面的法向量， 是偏置项， 是每个样本的标签， 是特征向量。

### 2. 模型训练
在实际应用中，我们通常使用现成的库，如 scikit-learn，来训练 SVM 模型。

训练过程包括选择合适的核（如线性、多项式、径向基函数等），调整参数（如 C, gamma）以及使用训练数据拟合模型。

### 3. Python 代码实现
以下是一个简单的 SVM 分类实例：

```
from sklearn import datasets
from sklearn.model_selection import train_test_split
from sklearn.svm import SVC
import matplotlib.pyplot as plt
import numpy as np

# 加载数据集
iris = datasets.load_iris()
X = iris.data[:, :2]  # 只取前两个特征
y = iris.target

# 划分训练集和测试集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# 创建 SVM 分类器
clf = SVC(kernel='linear')
clf.fit(X_train, y_train)

# 可视化
def make_meshgrid(x, y, h=.02):
x_min, x_max = x.min() - 1, x.max() + 1
y_min, y_max = y.min() - 1, y.max() + 1
xx, yy = np.meshgrid(np.arange(x_min, x_max, h),
np.arange(y_min, y_max, h))
return xx, yy

def plot_contours(ax, clf, xx, yy, **params):
Z = clf.predict(np.c_[xx.ravel(), yy.ravel()])
Z = Z.reshape(xx.shape)
out = ax.contourf(xx, yy, Z, **params)
return out

X0, X1 = X_train[:, 0], X_train[:, 1]
xx, yy = make_meshgrid(X0, X1)

fig, ax = plt.subplots()
plot_contours(ax, clf, xx, yy, cmap=plt.cm.coolwarm, alpha=0.8)
ax.scatter(X0, X1, c=y_train, cmap=plt.cm.coolwarm, s=20, edgecolors='k')
ax.set_ylabel('Sepal width')
ax.set_xlabel('Sepal length')
ax.set_xticks(())
ax.set_yticks(())
plt.show()

```
这段代码首先加载鸢尾花数据集，然后使用 SVM 的线性核进行分类，并在二维平面上可视化决策边界。

![](https://mmbiz.qpic.cn/mmbiz_png/jnpzoaI9v1uAU44NIt03vrx7RjvbaXmYRhcQwb9AIafVQVENuvAk2Qomqw0Z3VbnxTxWumG6oDgSgj8pQ6AiaaA/640?wx_fmt=png)

### 4. 实际效果
执行上述代码后，您将看到一个包含数据点和 SVM 决策边界的图形。这个图形直观地展示了 SVM 如何在二维空间中划分不同类别。

### 最后
SVM 能够有效处理线性和非线性数据。通过选择合适的核函数和调整参数，SVM 可以在各种数据集上实现高效的分类。

**每天一个简单通透的小案例，如果你对类似于这样的文章感兴趣。**

**欢迎关注、点赞、转发~**
