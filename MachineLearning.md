# 机器学习

---

> 机器学习是一门通过编程让计算机**从数据中学习**的科学。

---

## 系统类型



### 一、

根据训练期间接收的监督数量和监督类型，分为四个主要类别：有监督学习、无监督学习、半监督学习和强化学习。

### 有监督学习

#### 概念

* 标签：在有监督学习中，提供给算法的包含所需解决方案的训练集。如垃圾邮件过滤器通过大量的已人为标定是否为垃圾的电子邮件进行训练，然后学习如何对新邮件（新实例）进行分类。
* 

#### 学习任务\用途

* 分类任务：如垃圾邮件过滤器。
* 回归：通过给定一组预测器的特征（例如车的里程、使用年限、品牌等）来预测一个目标数值（例如车的价格）。



---

### 无监督学习

无监督学习的训练数据都是未经标记的。

#### 概念





#### 学习任务\用途

* 聚类分组：
* 可视化和降维
* 异常检测
* 新颖性检测
* 关联规则学习

**异常检测与新颖性检测**

两者的区别在于：

* 异常检测学习训练集中的中心特征，将部分边缘特征理解为**异常值**。
* 新颖性检测，则是学习模式，并判断新数据**是否属于该模式**。新颖性检测的训练集中不含异常/新颖值，异常检测则相反。





---

### 半监督学习

只有部分数据是已标记的。通常是无监督算法和有监督算法相结合。





---

### 强化学习

它的学习系统（智能体）能够观察环境，做出选择（自行选择最佳策略），执行动作，并获得回报（或惩罚）。





---

### 二、

根据系统是否可以从传入的数据流中进行**增量学习**。

### 批量学习

系统无法进行增量学习，必须使用所有可用数据进行训练，需要耗费大量时间的计算资源。

通常进行**离线学习**（先训练系统，后直接投入生产环境，停止学习），并在出现新变化时重新训练出新版本的系统。故不适用于应对快速变化的数据（如预测股票）。

### 在线学习

循序渐进地给系统提供训练数据（单独或小批量地），逐步积累学习成果。使得每一步学习都很快速且便宜，在计算资源有限时是个很好的选择。

在线学习即模型经过训练后投入生产环境，仍然随着新数据的输入（接收**持续的数据流**如股票价格）而不断学习。

* 核外学习：对超大数据集（超出一台计算机的主存储器容量的数据），该算法每次只加载部分数据并开始针对这部分数据开始训练，随后不断循环分批训练直至全部数据训练完毕。注意，核外学习是**离线**完成的（不在实时系统上），应归类于**增量学习**。



---

### 三、

根据机器学习系统进行泛化（预测）的方式

### 基于实例学习

系统学习实例并通过使用相似度度量来比较新实例和已经学习的旧实例（或它们的子集），从而泛化新实例。



### 基于模型学习

构建这些实例的模型，然后使用该模型对新实例进行预测。

**模型选择与评估**

模型选择包括**选择模型的类型**和**完全指定它的架构**。

模型评估：

* 效用函数（适应度函数）：衡量模型的正向性能表现
* 成本函数：衡量模型的负向性能表现（如线性回归问题使用成本函数衡量线性模型的预测与训练实例之间的差距，并尽量使该差距最小化）。

**模型的训练**

通过所提供的训练样本及合适的算法（如线性回归算法），找出最符合提供数据的线性模型的参数，来尽量使模型对新数据做出好的预测。



---

## 主要挑战

训练数据的数量不足、训练数据不具代表性、低质量数据、无关特征、过拟合训练数据、欠拟合训练数据

**不具代表性：**

* 采样噪声：样本集太小，有非代表性数据被选中
* 采样偏差：样本集非常大而采样方式欠妥。如一种特殊类型的采样偏差——无反应偏差。

**特征工程**

* 定义：提取出一组好的用来训练的特征集的过程。
* 步骤：
  1. 特征选择
  2. 特征提取
  3. 通过收集新数据创建新特征



**过拟合解决方案**

* 简化模型：选择较少参数的模型（如选择线性模型代替高阶多项式模型），或者减少训练数据中的属性数量，又或者是约束模型（正则化）。
* 收集更多的训练数据
* 减少训练数据中的噪声（如修复数据错误和消除异常值）



---



## 机器学习十九问

**如何定义机器学习？**

机器学习是关于构建可以从数据中学习的系统。学习意味着在一定的性能指标下，在某些任务上会变得越来越好。

**机器学习在哪些问题上表现突出，你能给出四种类型吗？**

机器学习非常适合没有算法解答的复杂问题，它可以替代一系列需要手动调整的规则，来构建适应不断变化的环境的系统并最终帮助人类（例如，数据挖掘）

**什么是被标记的训练数据集？**

带标签的训练集是一个包含每个实例所需解决方案（也称为标签）的训练集

**最常见的两种监督学习任务是什么？**

两个最常见的有监督任务是回归和分类。

**你能举出四种常见的无监督学习任务吗？**

常见的无监督任务包括

1. 聚类
2. 可视化
3. 降维
4. 关联规则学习

**要让一个机器人在各种未知的地形中行走，你会使用什么类型的机器学习算法？**

如果我们想要机器人在各种未知的地形中学习行走，则强化学习可能会表现最好，因为这通常是强化学习要解决的典型问题。

也可以将强化学习问题表示为有监督学习或半监督学习问题，但这种情况不是很自然的想法

**要将顾客分成多个组，你会使用什么类型的算法？**

如果你不知道如何定义组，则可以使用聚类算法（无监督学习）将客户划分为相似客户集群。

但是，如果你知道你想要拥有哪些组，那么可以将每个组的许多实例提供给分类算法（有监督学习），并将所有客户分类到这些组中。

**你会将垃圾邮件检测的问题列为监督学习还是无监督学习？**

垃圾邮件检测是一个典型的有监督学习问题：向该算法提供许多电子邮件及其标签（垃圾邮件或非垃圾邮件）。

**什么是在线学习系统？**

与批量学习系统相反，在线学习系统能够进行增量学习。这使得它能够快速适应不断变化的数据和自动系统，并能够处理大量数据。

**什么是核外学习？**

核外算法可以处理无法容纳在计算机主内存中的大量数据。核外学习算法将数据分成小批量，并使用在线学习技术从这些小批量数据中学习。

**什么类型的学习算法依赖相似度来做出预测？**

基于实例的学习系统努力通过死记硬背来学习训练数据。然后，当给定一个新的实例时，它将使用相似性度量来查找最相似的实例，并利用它们来进行预测。

**模型参数与学习算法的超参数之间有什么区别？**

一个模型具有一个或多个模型参数，这些参数确定在给定一个新实例的情况下该模型将预测什么（例如，线性模型的斜率）。

一种学习算法试图找到这些参数的最优值，以使该模型能很好地泛化到新实例。超参数是学习算法本身的参数，而不是模型的参数（例如，要应用正则化的数量）

**基于模型的学习算法搜索的是什么？它们最常使用的策略是什么？它们如何做出预测？**

是什么：基于模型的学习算法搜索模型参数的最优值，以便模型可以很好地泛化到新实例。

策略：我们通常通过最小化成本函数来训练这样的系统，该函数测量系统对训练数据进行预测时有多不准确，如果对模型进行了正则化则对模型复杂性要加上惩罚。

如何预测：为了进行预测，我们使用学习算法找到的模型参数值，再将新实例的特征输入到模型的预测函数中。

**你能给出机器学习中的四个主要挑战吗？**

机器学习中的一些主要挑战是数据的缺乏、数据质量差、数据的代表性不足、信息量不足、模型过于简单而欠拟合训练数据以及模型过于复杂而过拟合数据

**如果模型在训练数据上表现很好，但是应用到新实例上的泛化结果却很糟糕，是怎么回事？能给出三种可能的解决方案吗？**

如果模型在训练数据上表现出色，但在新实例上的泛化效果很差，则该模型可能会过拟合训练数据（或者我们在训练数据上非常幸运）：**过拟合**

过拟合的可能解决方法是：

- 获取更多数据
- 简化模型（选择更简单的算法，减少使用的参数或特征的数量，或对模型进行正则化）
- 减少训练数据中的噪声

**什么是测试集，为什么要使用测试集？**

测试数据集是用于在启动生产环境之前，估计模型在新实例上产生的泛化误差。

**验证集的目的是什么？**

验证集是用于比较模型，这样就可以选择最佳模型并调整超参数。

**什么是train-dev集，什么时候需要它，怎么使用？**

当训练数据集与验证数据集和测试数据集中使用的数据之间不匹配时，可以使用train-dev集（该数据集应始终与模型投入生产环境后使用的数据尽可能接近）。

**train-dev集是训练集的一部分（模型未在其上训练过）**。该模型在训练集的其他部分上进行训练，并在train-dev集和验证集上进行评估。

- 如果模型在训练集上表现良好，但在train-dev集上表现不佳，则该模型可能过拟合训练集。
- 如果它在训练集和train-dev集上均表现良好，但在验证集上却表现不佳，那么训练数据与验证数据和测试数据之间可能存在明显的数据不匹配，你应该尝试改善训练数据，使其看起来更像验证数据和测试数据。

**如果你用测试集来调超参数会出现什么错误？**

如果使用测试集来调整超参数，则可能会过拟合测试集，而且所测得的泛化误差会过于乐观（你可能会得到一个性能比预期差的模型）



---

## 项目流程

以使用美国加州人口普查的数据建立起预测加州房价中位数的模型。

### 观察大局

* 框架问题

  1. 明确业务目标（即知晓该模型将投入到机器学习流水线的哪一部分）；
  2. 了解当前的解决方案，试图从中获取有助于训练模型的数据；
  3. 最终确定机器学习系统的**类型**，明确系统的**学习任务**（如由 m 个特征值来预测 n 个目标值的 n 元 m 重回归问题）

  该例中问题是由含多种人口普查数据得到房价中位数这一个目标值，故为一元多重回归问题。

* 选择性能指标

  以回归问题为例：

  1. **均方根误差**（RMSE），又称标准误差 —— 对于较大的误差值，其权重较高
  2. **平均绝对误差**（MAE），又称平均绝对偏差 —— 适用于数据中存在许多异常区域的情况

  以上两种都是测量两个向量之间距离的方法，分别与 欧几里得范数L2 和 曼哈顿范数L1 对应，同样还有 Lk 范数

   `||Vk|| = (|V0|^k + |V1|^k + ··· + |Vk|^k) ^ (1 / k)`，范数指标越高，就对大数值越敏感，对小数值越不敏感。因此 RMSE 对异常值比 MAE 更受影响，但是，当离群值呈指数形式稀有时（如钟形曲线）。

​	

### 获得数据

1. 制作从网络或私人数据库爬取所需数据的脚本，又或者自行抽样调查（随机抽样、分层抽样）；

   ```python
   from pathlib import Path
   import pandas as pd
   import tarfile
   import urllib.request
   
   def load_housing_data():
       
       # Path(str1, str2, ..., ) 直接将字符串按由根目录到子目录的顺序转化为路径表示
       # 注意，字符串中也可以用 '/' 提前拼接部分地址，最后输出时会自动转化为 '\' 
       tarball_path = Path("datasets/housing.tgz")
       
       
       if not tarball_path.is_file():								 # 判断该文件是否存在
           Path("datasets").mkdir(parents=True, exist_ok=True)		    # 创建给定路径的目录
           url = "https://github.com/ageron/data/raw/main/housing.tgz" # tgz文件的直接下载地址
           urllib.request.urlretrieve(url, tarball_path)			   # 将通过 url 下载得到的文件存入 tarball_path 中
           
           # with···as···语法常用于打开文件并将返回的文件对象命名为 as 后的变量名
           with tarfile.open(tarball_path) as housing_tarball:      
               housing_tarball.extractall(path="datasets")			   # extractall 解压压缩包中的所有文件至指定路径的指定文件夹下，若无指定文件夹则以压缩包名字命名
               
       return pd.read_csv(Path("datasets/housing/housing.csv"))	   # 读取并返回 csv 文件对象
   ```

2. 快速查看数据结构，了解数据所含属性；

   ```python
   housing = load_housing_data() # 获取数据对象
   # 简单浏览数据集
   housing.head()						    # 展示数据集中的前五个数据
   housing.info()						    # 展示各属性的数据量和数据类型
   housing["ocean_proximity"].value_counts() # 计数所有该属性的所有数值的出现次数（由多到少排列）
   housing.describe()						# 所有属性的基础数据一览（包括最大最小值、平均值、数量、标准误差等）
   ```

   保存和展示图片

   ```python
   IMAGES_PATH = Path() / "images" / "end_to_end_project" # 保存至当前路径下 images 文件夹中的 end_to_end_project 文件夹
   IMAGES_PATH.mkdir(parents=True, exist_ok=True)		   # 创建以上目录
   
   def save_fig(fig_id, tight_layout=True, fig_extension="png", resolution=300):
       path = IMAGES_PATH / f"{fig_id}.{fig_extension}"
       if tight_layout:
           plt.tight_layout()
       plt.savefig(path, format=fig_extension, dpi=resolution)
       
       
   import matplotlib.pyplot as plt
   
   # extra code – the next 5 lines define the default font sizes
   plt.rc('font', size=14)
   plt.rc('axes', labelsize=14, titlesize=14)
   plt.rc('legend', fontsize=14)
   plt.rc('xtick', labelsize=10)
   plt.rc('ytick', labelsize=10)
   
   housing.hist(bins=50, figsize=(12, 8))
   save_fig("attribute_histogram_plots")  # extra code
   plt.show()							 # 展示图片
   ```

   

3. *创建测试集，通常是随机选择数据集中**20%**的实例，若数据集非常大，则可以适当缩小该比例。

   常见几种选取方案:

   1. 常用：每个实例都使用一个**标识符**来决定是否进入测试集，如计算每个实例标识符的哈希值，选取哈希值小于或等于最大哈希值20%的实例加入测试集。若以行索引作为唯一标识符，则必须保证新数据在数据集的末尾添加，并且不会删除任何一行数据。关于标识符的选择，通常需要选择一个最稳定的特征来创建，如不变的位置数据等。

      ```python
      # 按行索引作为标识符
      from zlib import crc32
      
      def is_id_in_test_set(identifier, test_ratio):
          return crc32(np.int64(identifier)) < test_ratio * 2**32
      
      def split_data_with_id_hash(data, test_ratio, id_column):
          ids = data[id_column]
          in_test_set = ids.apply(lambda id_: is_id_in_test_set(id_, test_ratio))
          return data.loc[~in_test_set], data.loc[in_test_set]
      
      housing_with_id = housing.reset_index()  # adds an `index` column
      train_set, test_set = split_data_with_id_hash(housing_with_id, 0.2, "index")
      
      housing_with_id["id"] = housing["longitude"] * 1000 + housing["latitude"] # 以经纬度信息区分
      train_set, test_set = split_data_with_id_hash(housing_with_id, 0.2, "id")
      
      from sklearn.model_selection import train_test_split
      train_set, test_set = train_test_split(housing, test_size=0.2, random_state=42)
      
      test_set["total_bedrooms"].isnull().sum()
      ```
   
      
   
   2. 第一次运行程序后立即保存测试集并置其于一旁。
   
   3. 导入 `import numpy as np` ，先设置随机数种子（如果不设置种子，函数会使用系统时间作为随机数生成的种子）如 `np.random.seed(42)` ，然后调用 `np.random.permutation()`，函数接受一个数组作为输入，并返回一个打乱顺序的新数组。并且注意种子值必须固定下来，以保证调试时能够复现同一种情况。
   
      ```python
      import numpy as np
      
      def shuffle_and_split_data(data, test_ratio):
          shuffled_indices = np.random.permutation(len(data))
          test_set_size = int(len(data) * test_ratio)
          test_indices = shuffled_indices[:test_set_size]
          train_indices = shuffled_indices[test_set_size:]
          return data.iloc[train_indices], data.iloc[test_indices]
      
      train_set, test_set = shuffle_and_split_data(housing, 0.2)
      
      # 确认训练集和测试集的数据量
      len(train_set) 
      len(test_set)
      ```
   
   
   评估所选测试集的合理性
   
   1. 评估抽样误差
   
      ```python
      # 为了找出当人口的女性比例为51.1%时，1000人的随机样本中女性比例低于48.5%或高于53.5%的概率
      # 法1：我们使用了二项式分布。二项式分布的cdf（）方法给出了女性数量等于或小于给定值的概率。
      from scipy.stats import binom
      
      sample_size = 1000
      ratio_female = 0.511
      proba_too_small = binom(sample_size, ratio_female).cdf(485 - 1)
      proba_too_large = 1 - binom(sample_size, ratio_female).cdf(535)
      print(proba_too_small + proba_too_large)
      # 0.10736798530929946
      # 发现有 10.7% 的概率会从测试集中获得差的样本
      
      
      
      #法2：直接模拟，乱序后取前 1000 个样例并统计
      np.random.seed(42)
      samples = (np.random.rand(100_000, sample_size) < ratio_female).sum(axis=1)
      ((samples < 485) | (samples > 535)).mean() # 求平均值
      # 0.1071
      # 同样发现有 10.7% 的概率会从测试集中获得差的样本
      ```
   
      若离群样品抽取概率过大，则需要重新制定抽样计划或者重新获取测试集。
   
   2. 咨询专家意见或自行思考与目标值**很可能**有较大相关性的属性特征，若非数据中已有属性，则需要创建新属性。随即以合适的方式在整个数据集中进行抽样（如分层抽样）得到测试集，然后统计并评估该测试集在该属性上表现是否与整个数据集一致。
   
      ```python
      # 如该案例下，从专家得知收入中位数与房价中位数联系非常密切，则在数据集副本中创建收入中位数这一属性
      housing["income_cat"] = pd.cut(housing["median_income"],
                                     bins=[0., 1.5, 3.0, 4.5, 6., np.inf],
                                     labels=[1, 2, 3, 4, 5])
      
      housing["income_cat"].value_counts().sort_index().plot.bar(rot=0, grid=True)
      plt.xlabel("Income category")
      plt.ylabel("Number of districts")
      save_fig("housing_income_cat_bar_plot")  # extra code - save the picture
      plt.show()
      
      # 按收入中位数分层抽样
      from sklearn.model_selection import StratifiedShuffleSplit
      
      splitter = StratifiedShuffleSplit(n_splits=10, test_size=0.2, random_state=42)
      strat_splits = []
      for train_index, test_index in splitter.split(housing, housing["income_cat"]):
          strat_train_set_n = housing.iloc[train_index]
          strat_test_set_n = housing.iloc[test_index]
          strat_splits.append([strat_train_set_n, strat_test_set_n])
          
      strat_train_set, strat_test_set = strat_splits[0]
      ```
   
      ```python
      # 更简便的写法
      strat_train_set, strat_test_set = train_test_split(
          housing, test_size=0.2, stratify=housing["income_cat"], random_state=42) # 分层抽样
      
      strat_test_set["income_cat"].value_counts() / len(strat_test_set)
      # 显示分层抽样得到的测试集的收入类别比例分布
      ```
   
      extra：比较总数据集、分层抽样集、随机抽样集三者的收入中位数分布情况与偏差
   
      ```python
      def income_cat_proportions(data):
          return data["income_cat"].value_counts() / len(data)
      
      train_set, test_set = train_test_split(housing, test_size=0.2, random_state=42)
      
      compare_props = pd.DataFrame({
          "Overall %": income_cat_proportions(housing),
          "Stratified %": income_cat_proportions(strat_test_set),
          "Random %": income_cat_proportions(test_set),
      }).sort_index()
      compare_props.index.name = "Income Category"
      compare_props["Strat. Error %"] = (compare_props["Stratified %"] /
                                         compare_props["Overall %"] - 1)
      compare_props["Rand. Error %"] = (compare_props["Random %"] /
                                        compare_props["Overall %"] - 1)
      (compare_props * 100).round(2)
      ```
   
      最后，若不是对副本进行操作，则必须要对原训练集和原测试集的数据进行恢复操作
   
      ```python
      # 从数据中剔除收入类别属性，恢复数据
      for set_ in (strat_train_set, strat_test_set):
          set_.drop("income_cat", axis=1, inplace=True)
      ```
   
      
   
   



### 从数据探索和可视化中获得洞见

创建一个训练集的**副本**（若训练集足够大，则直接从中抽样出一个**探索集**也可），并直接在其上操作而不损坏原训练集。

```python
housing = strat_train_set.copy() # 创建副本
```

* 将**地理数据**（或其他数据）可视化。若发现目标值与地理位置（如靠近海边）有关，通用办法是使用**聚类算法**来检测主集群，然后再为各个集群中心添加一个新的**衡量邻近距离**的特征（如海洋邻近度），但也要注意目标值不一定就与地理位置较强地相关。

  ```python
  # 几种可视化方法：
  # 1.最粗糙
  housing.plot(kind="scatter", x="longitude", y="latitude", grid=True)
  save_fig("bad_visualization_plot")  # extra code
  plt.show()
  
  # 2.优化后
  # 设置 alpha 值，更清楚地看到高密度数据点
  housing.plot(kind="scatter", x="longitude", y="latitude", grid=True, alpha=0.2)
  save_fig("better_visualization_plot")  # extra code
  plt.show()
  
  # 3.更优秀的可视化
  # 使用名叫 jet 的预定义颜色表来衡量数据点密度
  housing.plot(kind="scatter", x="longitude", y="latitude", grid=True,
               s=housing["population"] / 100, label="population",
               c="median_house_value", cmap="jet", colorbar=True,
               legend=True, sharex=False, figsize=(10, 7))
  save_fig("housing_prices_scatterplot")  # extra code
  plt.show()
  
  # 4.结合当地地理情况的可视化
  # 下载美国加州区域地图，并以此为底图，其余与法3一致。
  filename = "california.png"
  if not (IMAGES_PATH / filename).is_file():
      homl3_root = "https://github.com/ageron/handson-ml3/raw/main/"
      url = homl3_root + "images/end_to_end_project/" + filename
      print("Downloading", filename)
      urllib.request.urlretrieve(url, IMAGES_PATH / filename)
  
  housing_renamed = housing.rename(columns={
      "latitude": "Latitude", "longitude": "Longitude",
      "population": "Population",
      "median_house_value": "Median house value (ᴜsᴅ)"})
  housing_renamed.plot(
               kind="scatter", x="Longitude", y="Latitude",
               s=housing_renamed["Population"] / 100, label="Population",
               c="Median house value (ᴜsᴅ)", cmap="jet", colorbar=True,
               legend=True, sharex=False, figsize=(10, 7))
  
  california_img = plt.imread(IMAGES_PATH / filename)
  axis = -124.55, -113.95, 32.45, 42.05
  plt.axis(axis)
  plt.imshow(california_img, extent=axis) # 绘制底图
  
  save_fig("california_housing_prices_plot")
  plt.show()
  ```

* 寻找**相关性**：可通过 `corr()` 方法（数据集不是特别大时使用）计算出每对属性之间的**标准相关系数**，则可以获得与目标值相关性较强的某些属性。而属性之间的这些相关性，也可以通过 `pandas`的 `scatter_matrix()` 函数将相关性可视化展示。

  ```python
  # 计算每对属性之间的标准相关系数
  corr_matrix = housing.corr() 
  # 注：此处将原始数据中 ocean_proximity 属性部分的字符串表达全部对应替换成 -1 ~ -5 才可运行，
  # 因为 pandas 的 corr() 函数会将数字和数字字符串转化为浮点型，但如果识别到不可转换的字符串则会报错
  
  # 得到每个属性与目标值房价中位数的相关性
  corr_matrix["median_house_value"].sort_values(ascending=False)
  
  # 可视化上述相关性
  from pandas.plotting import scatter_matrix
  
  attributes = ["median_house_value", "median_income", "total_rooms",
                "housing_median_age"]
  scatter_matrix(housing[attributes], figsize=(12, 8))
  save_fig("scatter_matrix_plot")  # extra code
  plt.show()
  
  # 将相关性表现较好的单拎出来，发现最有潜力能够预测房价中位数的属性是收入中位数
  housing.plot(kind="scatter", x="median_income", y="median_house_value",
               alpha=0.1, grid=True)
  save_fig("income_vs_house_value_scatterplot")  # extra code
  plt.show()
  
  # 至此，得到结论：已有属性中契合度最好的是收入中位数
  ```
  
  此外，还可以试验**不同属性的组合情况**，比如以某两个属性的**比值**作为一个新属性，并重新评估其与目标值的相关性，很多时候会发现其相关性会高于原本两个属性各自贡献的相关性。
  
  ```python
  # 可以通过某种方式（如比值）组合不同的属性得到的新属性，重新评估其与目标值的价值，通常得到的属性与目标值的相关性更强
  housing["rooms_per_house"] = housing["total_rooms"] / housing["households"]
  housing["bedrooms_ratio"] = housing["total_bedrooms"] / housing["total_rooms"]
  housing["people_per_house"] = housing["population"] / housing["households"]
  
  corr_matrix = housing.corr()
  corr_matrix["median_house_value"].sort_values(ascending=False)
  # 发现组合属性的表现并不理想（虽然比原单属性的表现更好），契合度远远低于收入中位数，故作罢
  ```
  
  此时，你应当找到与目标值具有较强相关性的若干个属性或属性组合，并从中继续着手研究。
  
  

### 数据准备

首先确保训练集是原始数据，然后分离训练集中的预测器和标签（因为我们不一定对它们使用相同的转换方式）以便单独对其一进行操作。

```python
# drop() 返回一个缺少标签值 "median_house_value" 的 strat_train_set 副本，注意并不会实际影响到 strat_train_set
housing = strat_train_set.drop("median_house_value", axis=1)

# 复制一份标签
housing_labels = strat_train_set["median_house_value"].copy()
```

* **数据清理**

  **异常值检验**

  以下是一些常见的异常值类型：

  1. 离群值（Outliers）：离群值是指与其他数据点明显不同的值。它们可能是由于测量误差、异常情况、录入错误等原因导致的。离群值可能是极端值，也可能是偏离常规分布的值。
  2. 错误数据（Erroneous Data）：错误数据是指由于系统错误或人为错误而导致的不正确数据。这可能包括错误的测量、传感器失灵、数据录入错误等。
  3. 噪声（Noise）：噪声是指在数据中存在的不相关或随机的扰动。虽然噪声不一定表示异常，但它可能与真实数据之间的差异过大导致误判异常。
  4. 缺失值（Missing Values）：缺失值是指数据集中某些特征或属性缺少数值。这种缺失可能由于测量问题、人为错误或者其他原因导致，缺失值的存在可能会对机器学习模型的性能产生不良影响。
  5. 异常模式（Anomalous Patterns）：异常模式是指在数据中存在的与预期模式或常规模式不一致的特定模式。这可能包括数据分布的突变、时序中的异常趋势等。

  * 缺失值检验：由于大部分机器学习算法无法在缺失值的特征上工作，则需要创建一些函数来辅助算法进行下去。当某属性在部分样本中值缺失，现有三种解决方式（以下函数使用的均是 `pandas` 中的 `DataFrame`）：

    1. 放弃这些相应区域，即直接删除含属性值缺失的样本

       ```python
       housing.dropna(subset=["total_bedrooms"], inplace=True) 
       ```

    2. 放弃整个属性，即从所有样本中剔除该属性特征

       ```python
       housing.drop("total_bedrooms", axis=1)
       ```

    3. 将缺失值填充为某个值（0、平均数或中位数等）来代替“值缺失”这一状态

       ```python
       median = housing["total_bedrooms"].median()  		  # 计算中位数
       housing["total_bedrooms"].fillna(median, inplace=True) # 并用中位数代替
       ```

       若需要用中位数替代缺失值，这里有更易操作的类。并且我们无法确定新数据是否一定不存在缺失值，故稳妥起见应将 `imputer` 应用于所有的数值属性，这样对于新数据中的缺失值均会自动替换为此时所得的中位数值。

       ```python
       from sklearn.impute import SimpleImputer
       
       imputer = SimpleImputer(strategy="median")
       
       # 中位数只能计算于数值属性上，而不能计算文本属性，故先创建一个只有数值属性的副本
       housing_num = housing.select_dtypes(include=[np.number])
       
       # 使用 fit() 方法将 imputer 实例适配到训练数据中（实际上就是开始计算数据中的中位数值）
       imputer.fit(housing_num)
       
       # 替换操作，将训练集转换
       X = imputer.transform(housing_num)
       
       # X 是一个 NumPy 数组，可以转化回为 pandas DataFrame
       housing_tr = pd.DataFrame(X, columns=housing_num.columns,
                                 index=housing_num.index)
       ```

       `imputer`计算每个属性的中位数值，并存储在实例变量 `statistics_` 中

       ```python
       # 两种方法均可查询各属性的中位数值
       imputer.statistics_
       
       housing_num.median().values
       
       # 查看所计算的所有属性名
       imputer.feature_names_in_
       
       # 查看计算的目标值类别（如中位数）
       imputer.strategy
       ```

  * 离群值检验：

    ```python
    # 离群值检验算法
    from sklearn.ensemble import IsolationForest
    
    isolation_forest = IsolationForest(random_state=42)
    outlier_pred = isolation_forest.fit_predict(X)
    
    outlier_pred
    
    # 删除离群值
    housing = housing.iloc[outlier_pred == 1]
    housing_labels = housing_labels.iloc[outlier_pred == 1]
    ```

  注意在之后评估系统时也要对测试集进行相同方式的清理

  

* **处理文本和分类属性**

  
