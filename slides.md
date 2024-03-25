---
theme: seriph
title: pattern-recognition-work
class: text-center
highlighter: shiki
drawings:
  persist: false
transition: slide-left
fonts:
  # 用于代码块、内联代码等
  mono: Fira Code
layout: cover
mdc: true
---

# 模式识别第一次作业

基于概率方法的故障数据检测{.font-size-6}

<div class="pt-10">李雨佳 曾道斌 曹家宇 张思涵</div>

<div class="abs-bl m-6 items-center flex gap-2">
  <a href="https://github.com/JYuCao/one-class-classification" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
  <a class="text-sm opacity-50" href="https://github.com/JYuCao/one-class-classification" target="_blank">https://github.com/JYuCao/one-class-classification</a>
</div>


---
transition: fade-out
layout: image
image: ./question.png
---

---
layout: default
---
# 方法一
基于多元正态分布拟合的故障数据检测

- 基于训练集拟合出多元正态分布
- 根据拟合密度函数计算训练集每个点的概率密度，并选取概率密度最小的一个点作为阈值
- 对测试集进行检测，概率密度低于阈值的点判定为异常点
- 检测结果，计算准确率
$$
多元正态分布：f(\mathbf{x}) = \frac{1}{\sqrt{(2\pi)^k|\boldsymbol\Sigma|}}
\exp\left(-\frac{1}{2}({\mathbf{x}}-\boldsymbol\mu)^T{\boldsymbol\Sigma}^{-1}({\mathbf{x}}-\boldsymbol\mu)
\right)
$$

```python{None|1-5|6-9|all}
train_data = np.genfromtxt("/path") # 读取训练集
train_cov = np.cov(train_data.T) # 计算训练集协方差矩阵
train_mean = np.mean(train_data, axis=0) # 计算训练集均值
# 拟合多元正态分布
train_mvn = multivariate_normal(mean=train_mean, cov=train_cov)
pdf_value = train_mvn.logpdf(train_data) # 计算训练集每个点的概率密度的对数值
threshold = np.min(pdf_value) # 选取概率密度最小的一个点作为阈值
test_data = np.genfromtxt("/path") # 读取测试集
train_mvn.pdf(test_data) # 计算测试集每个点的概率密度
```

---
layout: default
transition: slide-up
---
# 方法一
基于多元正态分布拟合的故障数据检测

<n-space>

<div class="text-sm w70">
  <div class="w-70 h-55">
    <NImage src="s1_1.png"></NImage>
  </div>
<br>
训练集
</div>

<div class="text-sm w70">
  <div class="w-70 h-55">
    <NImage src="s1_2.png"></NImage>
  </div>
<br>
测试集1

Currency in Test Data 1: 0.9995
</div>

<div class="text-sm w70">
  <div class="w-70 h-55">
    <NImage src="s1_3.png"></NImage>
  </div>
  <br>
  测试集2

  Currency in Test Data 2: 0.50075
</div>
</n-space>

---
layout: default
---
# 方法一（续）
基于马氏距离的故障数据检测

- 基于训练集计算出协方差矩阵与均值
- 计算出训练集每个点到均值的马氏距离
- 选取马氏距离最大的一个点作为阈值
- 以训练集的均值和协方差矩阵作为分布的估计值
- 对测试集进行检测，计算每个点到均值的马氏距离，大于于阈值的点判定为异常点

$$
马氏距离：D_M(\mathbf{x}) = \sqrt{(\mathbf{x} - \mathbf{\mu})^T \mathbf{\Sigma}^{-1} (\mathbf{x} - \mathbf{\mu})}
$$

```python{None|1-3|4-6|7-9|all}
train_data = np.genfromtxt("/path") # 读取训练集
train_cov = np.cov(train_data.T) # 计算训练集协方差矩阵
train_mean = np.mean(train_data, axis=0) # 计算训练集均值
# 计算训练集每个点到均值的马氏距离
m_dist = mahalanobis(train_data1[i], train_mean, np.linalg.inv(train_cov))
threshold = np.max(m_dist) # 选取马氏距离最大的一个点作为阈值
test_data = np.genfromtxt("/path") # 读取测试集
test_m_dist = mahalanobis(test_data1[i], train_mean, np.linalg.inv(train_cov))# 计算测试集每个点到均值的马氏距离
```

---
layout: default
transition: slide-up
---
# 方法一（续）
基于马氏距离的故障数据检测

<n-space>

<div class="text-sm w70">
  <div class="w-70 h-70">
    <NImage src="s2_1.png"></NImage>
  </div>
<br>
训练集
threshold = 8.2
</div>

<div class="text-sm w70">
  <div class="w-70 h-70">
    <NImage src="s2_2.png"></NImage>
  </div>
<br>
测试集1

threshold = 8.2, Currency in Test Data 1: 0.99975
</div>

<div class="text-sm w70">
  <div class="w-70 h-70">
    <NImage src="s2_3.png"></NImage>
  </div>
  <br>
  测试集2

threshold = 8.2, Currency in Test Data 2: 0.59375
</div>
</n-space>

---
layout: default
---
# 方法二
OneClassSVM 单分类支持向量机

一种无监督的学习算法，主要用于异常检测。

基本思想：在特征空间中找到一个最小的超球体，使得大部分数据点都在这个超球体内部。

缺点：
1. 它的计算复杂度较高，对于大规模数据可能难以处理。{.font-size-3}
2. 它的性能受到参数选择的影响，需要通过交叉验证等方法来选择合适的参数。{.font-size-3}


```python{None|1|2-3|4-5|all}
train_data = np.genfromtxt("/path") # 读取训练集
clf = OneClassSVM(nu=0.1,kernel='rbf') # 初始化OneClassSVM，核函数选择rbf(径向基函数核)
clf.fit(train_data) # 训练模型
test_data = np.genfromtxt("/path") # 读取测试集
clf.predict(test_data) # 预测测试集, 返回-1表示异常点
```
<br>

> 算法细节不会，但是可以通过调用sklearn库来实现

---
layout: default
transition: slide-up
---
# 方法二
OneClassSVM 单分类支持向量机

<n-space>

<div class="text-sm w70">
  <div class="w-70 h-70">
    <NImage src="s3_1.png"></NImage>
  </div>
<br>
训练集
</div>

<div class="text-sm w70">
  <div class="w-70 h-70">
    <NImage src="s3_2.png"></NImage>
  </div>
<br>
测试集1

Currency in Test Data 1: 0.94775
</div>

<div class="text-sm w70">
  <div class="w-70 h-70">
    <NImage src="s3_3.png"></NImage>
  </div>
  <br>
  测试集2

Currency in Test Data 2: 0.5045
</div>
</n-space>

---
layout: default
---
# 方法三
混合高斯模型

- 基于训练集拟合出混合高斯模型
- 根据拟合密度函数计算训练集每个点的概率密度，并选取概率密度最小的一个点作为阈值
- 对测试集进行检测，概率密度低于阈值的点判定为异常点
- 检测结果，计算准确率

```python{None|1-3|4-5|6-7|all}
train_data = np.genfromtxt("/path") # 读取训练集
gmm = GaussianMixture(n_components=2) # 初始化混合高斯模型,n_components为高斯分布的个数
gmm.fit(train_data) # 训练模型
train_scores = gmm.score_samples(train_data) # 计算训练集每个点的概率密度
threshold = np.min(train_scores) # 选取概率密度最小的一个点作为阈值
test_data = np.genfromtxt("/path") # 读取测试集
gmm.score_samples(test_data) # 计算测试集每个点的概率密度
```

- n_components的取值会影响模型的拟合效果，需要通过交叉验证等方法来选择合适的参数

<br>

> 算法细节不会，但是可以通过调用sklearn库来实现

---
layout: default
---
# 方法三
混合高斯模型
<n-space>

<div class="text-sm w70">
  <div class="w-70 h-70">
    <NImage src="s4_1.png"></NImage>
  </div>
<br>
训练集
</div>

<div class="text-sm w70">
  <div class="w-70 h-70">
    <NImage src="s4_2.png"></NImage>
  </div>
<br>
测试集1

Currency in Test Data 1: 0.9995
</div>

<div class="text-sm w70">
  <div class="w-70 h-70">
    <NImage src="s4_3.png"></NImage>
  </div>
  <br>
  测试集2

Currency in Test Data 2: 0.5025
</div>
</n-space>

---
layout: default
transition: slide-up
---
# 方法三
混合高斯模型

- n_components的取值会影响模型的拟合效果，需要通过交叉验证等方法来选择合适的参数

```python
gmm = GaussianMixture(n_components=2)
```
<n-space>
<div class="text-sm w100">
  <div class="w-80 h-70">
    <NImage src="d1.png"></NImage>
  </div>
训练集1
</div>

<div class="text-sm w70">
  <div class="w-80 h-70">
    <NImage src="d2.png"></NImage>
  </div>
训练集2
</div>
</n-space>

**通过实验,发现当n_components取到与独立的变量即分布数量相近时效果最好**

---
layout: default
---
# 方法四
parzen窗法

- 选取高斯核函数
- 基于训练集拟合出概率密度模型
- 根据拟合密度函数计算训练集每个点的概率密度，并选取概率密度最小的一个点作为阈值
- 对测试集进行检测，概率密度低于阈值的点判定为异常点
- 检测结果，计算准确率

```python{None|1-3|4-5|6-7|all}
train_data = np.genfromtxt("/path") # 读取训练集
kde = KernelDensity(kernel='gaussian', bandwidth=1.0) # 初始化核密度估计模型
kde.fit(train_data) # 训练模型
pdf_value = kde.score_samples(train_data) # 计算训练集每个点的概率密度的对数值
threshold = np.min(pdf_value) # 选取概率密度最小的一个点作为阈值
test_data = np.genfromtxt("/path") # 读取测试集
kde.score_samples(test_data) # 计算测试集每个点的概率密度
```

> 算法通过调用sklearn库来实现


---
layout: default
transition: fade-out
---
# 方法四
parzen窗法

<n-space>

<div class="text-sm w70">
  <div class="w-70 h-70">
    <NImage src="s5_1.png"></NImage>
  </div>
<br>
训练集
</div>

<div class="text-sm w70">
  <div class="w-70 h-70">
    <NImage src="s5_2.png"></NImage>
  </div>
<br>
测试集1

Currency in Test Data 1: 1.0
</div>

<div class="text-sm w70">
  <div class="w-70 h-70">
    <NImage src="s5_3.png"></NImage>
  </div>
  <br>
  测试集2

Currency in Test Data 2: 1.0
</div>
</n-space>

**🧑‍💻 这个方法效果很好, 而且bandwidth取值很大范围内都能做到这么好的结果, 已经不知道怎么解释了.**

---
layout: cover
---

# Thanks
<div class="absolute top-3/5 left-1/2 transform -translate-x-1/2 -translate-y-1/2  items-center flex gap-2">
  <a href="https://github.com/JYuCao/one-class-classification" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
  <a class="text-sm opacity-50" href="https://github.com/JYuCao/one-class-classification" target="_blank">https://github.com/JYuCao/one-class-classification</a>
</div>