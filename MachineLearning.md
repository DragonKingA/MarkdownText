> 机器学习是一门通过编程让计算机**从数据中学习**的科学。

# 系统类型

根据训练期间接收的监督数量和监督类型，分为四个主要类别：有监督学习、无监督学习、半监督学习和强化学习。

---

## 有监督学习

概念

* 标签：在有监督学习中，提供给算法的包含所需解决方案的训练集。如垃圾邮件过滤器通过大量的已人为标定是否为垃圾的电子邮件进行训练，然后学习如何对新邮件（新实例）进行分类。
* 

学习任务\用途

* 分类任务：如垃圾邮件过滤器。
* 回归：通过给定一组预测器的特征（例如车的里程、使用年限、品牌等）来预测一个目标数值（例如车的价格）。



---

## 无监督学习

无监督学习的训练数据都是未经标记的。

概念





学习任务\用途

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

## 半监督学习

只有部分数据是已标记的。通常是无监督算法和有监督算法相结合。





---

## 强化学习

它的学习系统（智能体）能够观察环境，做出选择（自行选择最佳策略），执行动作，并获得回报（或惩罚）。





---

根据系统是否可以从传入的数据流中进行**增量学习**。

## 批量学习

系统无法进行增量学习，必须使用所有可用数据进行训练，需要耗费大量时间的计算资源。

通常进行**离线学习**（先训练系统，后直接投入生产环境，停止学习），并在出现新变化时重新训练出新版本的系统。故不适用于应对快速变化的数据（如预测股票）。

## 在线学习

循序渐进地给系统提供训练数据（单独或小批量地），逐步积累学习成果。使得每一步学习都很快速且便宜，在计算资源有限时是个很好的选择。

在线学习即模型经过训练后投入生产环境，仍然随着新数据的输入（接收**持续的数据流**如股票价格）而不断学习。

* 核外学习：对超大数据集（超出一台计算机的主存储器容量的数据），该算法每次只加载部分数据并开始针对这部分数据开始训练，随后不断循环分批训练直至全部数据训练完毕。注意，核外学习是**离线**完成的（不在实时系统上），应归类于**增量学习**。



---

根据机器学习系统进行泛化（预测）的方式

## 基于实例学习

系统学习实例并通过使用相似度度量来比较新实例和已经学习的旧实例（或它们的子集），从而泛化新实例。



## 基于模型学习

构建这些实例的模型，然后使用该模型对新实例进行预测。

**模型选择与评估**

模型选择包括**选择模型的类型**和**完全指定它的架构**。

模型评估：

* 效用函数（适应度函数）：衡量模型的正向性能表现
* 成本函数：衡量模型的负向性能表现（如线性回归问题使用成本函数衡量线性模型的预测与训练实例之间的差距，并尽量使该差距最小化）。

**模型的训练**

通过所提供的训练样本及合适的算法（如线性回归算法），找出最符合提供数据的线性模型的参数，来尽量使模型对新数据做出好的预测。



---

# 主要挑战

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



# 机器学习十九问

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

# 项目流程

以使用美国加州人口普查的数据建立起预测加州房价中位数的模型。

## 观察大局

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

## 获得数据

1. 制作从公共网络或私人数据库爬取所需数据的脚本；

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

2. 快速查看数据结构，了解数据所含属性及其类型；

   ```python
   housing = load_housing_data() # 获取数据对象
   # 简单浏览数据集
   housing.head()						    # 展示数据集中的前五个数据
   housing.info()						    # 展示各属性的数据量和数据类型
   housing["ocean_proximity"].value_counts() # 计数所有该属性的所有数值的出现次数（由多到少排列）
   housing.describe()						# 所有属性的基础数值数据一览（包括最大最小值、平均值、数量、标准误差等）
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

   

3. **创建测试集**，通常是随机选择数据集中**20%**的实例，若数据集非常大，则可以适当缩小该比例。

   从数据集中选取测试集（多种方案）

   1. 常用方法：每个实例都使用一个**标识符**来决定是否进入测试集，如计算每个实例标识符的哈希值，选取哈希值小于或等于最大哈希值20%的实例加入测试集。若以行索引作为唯一标识符，则必须保证新数据在数据集的末尾添加，并且不会删除任何一行数据。关于标识符的选择，通常需要选择一个最稳定的特征来创建，如不变的位置数据等。

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
   
   3. 导入 `import numpy as np` ，先设置随机数种子（如果不设置种子，函数会使用系统时间作为随机数生成的种子）如 `np.random.seed(42)` ，然后调用 `np.random.permutation()`，函数接受一个数组作为输入，并返回一个打乱顺序的新数组。并且注意**种子值必须固定下来**，以保证调试时能够复现同一种情况。
   
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
      # 同样发现有 10.7% 的概率会从测试集中获得表现差的样本
      ```
   
      若表现差的样品的抽取概率过大，则需要重新制定抽样计划或者重新获取测试集。
   
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
   





## 从数据探索和可视化中获得洞见

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
  
  此时，你应当找到与目标值具有较强相关性的若干个属性或属性组合。
  
  

## 数据准备

首先确保训练集是原始数据，然后**分离**训练集中的预测器和标签（因为我们不一定对它们使用相同的转换方式）以便单独对其一进行操作。

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

  注意在之后评估系统时也要对测试集进行相同方式的数据清理



* **处理文本和分类属性**

  由于大部分机器学习算法更倾向于对数值属性进行处理和学习，因此通常将文本属性转化为相应的数值属性方便算法学习，而选择哪种编码方法取决于具体的数据特征和机器学习算法的要求。

  ```python
  # 可预先粗略了解文本属性的某些值
  housing_cat = housing[["ocean_proximity"]]
  housing_cat.head(8) # 前 8 个样本
  ```

  * 方法一：**OrdinalEncoder类**

    一般方法是使用 `Scikit-Learn` 的 `OrdinalEncoder` 类将文本值等不便处理的数据类型转化为**连续整数编码**。

    ```python
    # 转换为整数编码
    from sklearn.preprocessing import OrdinalEncoder
    
    ordinal_encoder = OrdinalEncoder()
    housing_cat_encoded = ordinal_encoder.fit_transform(housing_cat)
    
    # 展示前 8 个样本的该属性（若有多个，则以 '.' 为分隔符展示在一个数组中，数组之间则以 ',' 为分隔符）对应替换后的整数编码
    housing_cat_encoded[:8]
    
    # 查看类别列表（每个类比属性对应一个一维数组，存储该类别属性的所有可能的文本值）
    ordinal_encoder.categories_
    ```

    特点：这种编码顾名思义是有顺序的，即不同的文本值之间存在一个固定的顺序。例如，将"低"、“中”、"高"这样的文本属性编码为0、1、2。这种编码适用于文本属性之间**有一定顺序关系**的情况，可以保留文本属性之间的相对顺序信息。

  * 方法二：**独热编码**

    定义：即 One-Hot 编码，又称一位有效编码。其方法是使用 N位 状态寄存器来对 N个状态 进行编码，每个状态都有它独立的寄存器位，并且在任意时候，其中**只有一位有效**。One-Hot 编码是**分类变量**作为**二进制向量**的表示。

    例如，某个属性有四种状态，那么将用四位二进制来表示，如 0001 表示该属性处于第一种状态。对于一个样本，若有三种各有四种状态的属性，如 `[1000, 0001, 0010]` 其独热编码表示为 `100000010010` 一串二进制序列。

    特点：这种编码适用于文本属性之间**没有顺序关系**的情况，可以将文本属性转换为机器学习算法更容易处理的格式，用于表示文本属性的存在与否。。

    优点：解决了 **分类器不好处理离散数据** 的问题，在一定程度上也起到了 **扩充特征** 的作用。

    缺点：

    1. 它是一个词袋模型，不考虑 **词与词之间的顺序**（文本中词的顺序信息也是很重要的）；
    2. 它 **假设词与词相互独立**（在大多数情况下，词与词是相互影响的）；
    3. 它得到的 **特征是离散稀疏** 的 (这个问题最严重)。

    编码方式：

    * 方法一：`scikit-learn`库的`OneHotEncoder`类——该类提供了更灵活的控制选项，可以处理多个特征列和处理稀疏矩阵等情况。

      ```python
      # 写法1：
      # 转换为独热编码
      from sklearn.preprocessing import OneHotEncoder
      
      cat_encoder = OneHotEncoder()
      housing_cat_1hot = cat_encoder.fit_transform(housing_cat)
      housing_cat_1hot
      
      housing_cat_1hot.toarray() # 将 SciPy 稀疏矩阵转换为密集的 NumPy 数组，节省内存
      
      
      
      # 写法2：
      # 转换为独热编码时将 sparse 参数设置为 False 即可直接得到 NumPy 数组，可省去转换为密集矩阵的步骤
      cat_encoder = OneHotEncoder(sparse=False)
      housing_cat_1hot = cat_encoder.fit_transform(housing_cat)
      housing_cat_1hot
      ```

      ```python
      # 查看类别列表
      cat_encoder.categories_
      ```

    * 方法二：`pandas`库的`get_dummies()`函数——该函数可以将指定的列转换为独热编码的形式。

      ```python
      df_test = pd.DataFrame({"ocean_proximity": ["INLAND", "NEAR BAY"]}) # 选择该属性的这两列值
      pd.get_dummies(df_test)
      cat_encoder.transform(df_test) # 进行独热编码
      
      
      df_test_unknown = pd.DataFrame({"ocean_proximity": ["<2H OCEAN", "ISLAND"]}) # 选择该属性的这两列值
      pd.get_dummies(df_test_unknown)
      cat_encoder.handle_unknown = "ignore"  # 在转换过程中遇到未知分类特征时，是引发错误还是忽略（默认为引发，即参数默认为“error”）。当此参数设置为“ignore”并且在转换过程中遇到未知类别时，这一特征的 one-hot 编码列将全置为 0。在逆变换中，未知类别将表示为 None。
      cat_encoder.transform(df_test_unknown) # 进行独热编码
      
      
      cat_encoder.feature_names_in_       # 查询特征类名
      cat_encoder.get_feature_names_out() # 查询对应特征值（获取特征值列表）
      
      
      # 输出编码结果
      df_output = pd.DataFrame(cat_encoder.transform(df_test_unknown),
                               columns=cat_encoder.get_feature_names_out(),
                               index=df_test_unknown.index)
      df_output
      ```



* **自定义转换器**

  在这之前，需要知道的是，`Scikit-Learn` 提供了许多有用的转换器，当你需要完成自定义清理操作或组合特定属性等任务时同样可以利用它来编写自己的转换器。

  > `Scikit-Learn` 依赖于**鸭子类型**的编译，而不是继承。鸭子类型是一种动态类型的概念，其核心思想是**关注对象的行为**（方法和属性），而不是关注对象的具体类型或类别。只要一个对象拥有特定的方法和属性，那么它就可以被视为具有某个特定类型或类别的对象，无论其实际的类型是什么。
  >
  > 例如，在分类问题中，我们可以使用逻辑回归、支持向量机或决策树等不同的模型，但只要这些模型具有“训练”和“预测”等通用方法，我们就可以将它们视为可用于分类任务的模型。

  一般自定义操作为：
  
  * 利用`Scikit-Learn` 中的 `FunctionTransformer`方法，它第一个参数为单变元函数（只有一个自变量和一个因变量即 `y = f(x)`，故也可以是 Lambda 表达式），并将该函数转换为**可用于数据转换的转换器**
  
    ```python
    # 以下为一些简单的自定义转换器
    # 1. 转换为 log{e} 的值
    from sklearn.preprocessing import FunctionTransformer
    
    log_transformer = FunctionTransformer(np.log, inverse_func=np.exp) # 以 e 为底
    log_pop = log_transformer.transform(housing[["population"]])
    
    log_pop
    
    # 2. 转换为基于 rbf_kernel 核函数转换的值
    rbf_transformer = FunctionTransformer(rbf_kernel,
                                          kw_args=dict(Y=[[35.]], gamma=0.1))
    # kw_args 表示传递给前面函数的关键字参数，里面的关键字参数将指定前面函数的某些参数的值
    age_simil_35 = rbf_transformer.transform(housing[["housing_median_age"]])
    
    age_simil_35
    
    # 3. 转换为基于 rbf_kernel 核函数转换的值，并指定核函数的关键字参数
    sf_coords = 37.7749, -122.41
    sf_transformer = FunctionTransformer(rbf_kernel,
                                         kw_args=dict(Y=[sf_coords], gamma=0.1))
    sf_simil = sf_transformer.transform(housing[["latitude", "longitude"]])
    
    sf_simil
    
    # 4. 传入lambda函数
    ratio_transformer = FunctionTransformer(lambda X: X[:, [0]] / X[:, [1]]) # 第一列除以第二列的结果
    ratio_transformer.transform(np.array([[1., 2.], [3., 4.]]))
    ```
  
  * 创建一个类，编写`fit() -> 返回 self、transform()、fit_transform()` 这三种方法，还可以通过添加`Scikit-Learn` 中的 `TransformerMixin`作为基类直接获得方法 `fit_transform()`，并且若添加 `BaseEstimator` 作为基类（并在构造函数中避免 `*args` 和 `**kargs`）可以获得自动调整超参数的两种方法 `get_params()` 和 `set_params()`。
  
    ```python
    # 1.
    from sklearn.base import BaseEstimator, TransformerMixin
    from sklearn.utils.validation import check_array, check_is_fitted
    
    class StandardScalerClone(BaseEstimator, TransformerMixin):
        def __init__(self, with_mean=True):  # no *args or **kwargs!
            self.with_mean = with_mean
    
        def fit(self, X, y=None):  # y is required even though we don't use it
            X = check_array(X)  # checks that X is an array with finite float values
            self.mean_ = X.mean(axis=0)
            self.scale_ = X.std(axis=0)
            self.n_features_in_ = X.shape[1]  # every estimator stores this in fit()
            return self  # always return self!
    
        def transform(self, X):
            check_is_fitted(self)  # looks for learned attributes (with trailing _)
            X = check_array(X)
            assert self.n_features_in_ == X.shape[1]
            if self.with_mean:
                X = X - self.mean_
            return X / self.scale_
        
        
        
    # 2. K-均值聚类算法
    from sklearn.cluster import KMeans
    
    class ClusterSimilarity(BaseEstimator, TransformerMixin):
        def __init__(self, n_clusters=10, gamma=1.0, random_state=None): # 默认将数据分为 10 簇类
            self.n_clusters = n_clusters
            self.gamma = gamma
            self.random_state = random_state
    
        def fit(self, X, y=None, sample_weight=None):
            self.kmeans_ = KMeans(self.n_clusters, n_init = 'auto', random_state=self.random_state, algorithm = 'elkan') 
            # sklearn 或 numpy 的版本bug，需要指定  n_init = 'auto' 以及 algorithm = 'elkan' ，否则报错
            self.kmeans_.fit(X, sample_weight=sample_weight)
            return self  # always return self!
    
        def transform(self, X):
            return rbf_kernel(X, self.kmeans_.cluster_centers_, gamma=self.gamma)
        
        def get_feature_names_out(self, names=None):
            return [f"Cluster {i} similarity" for i in range(self.n_clusters)]
        
    cluster_simil = ClusterSimilarity(n_clusters=10, gamma=1., random_state=42)
    similarities = cluster_simil.fit_transform(housing[["latitude", "longitude"]],
                                               sample_weight=housing_labels)
    
    similarities[:3].round(2)
    
    # extra code – 显示图像： K-均值算法得到的 K 个聚类中心
    housing_renamed = housing.rename(columns={
        "latitude": "Latitude", "longitude": "Longitude",
        "population": "Population",
        "median_house_value": "Median house value (ᴜsᴅ)"})
    housing_renamed["Max cluster similarity"] = similarities.max(axis=1)
    
    housing_renamed.plot(kind="scatter", x="Longitude", y="Latitude", grid=True,
                         s=housing_renamed["Population"] / 100, label="Population",
                         c="Max cluster similarity",
                         cmap="jet", colorbar=True,
                         legend=True, sharex=False, figsize=(10, 7))
    plt.plot(cluster_simil.kmeans_.cluster_centers_[:, 1],
             cluster_simil.kmeans_.cluster_centers_[:, 0],
             linestyle="", color="black", marker="X", markersize=20,
             label="Cluster centers")
    plt.legend(loc="upper right")
    save_fig("district_cluster_plot")
    plt.show()
    ```



* **特征缩放**

  当输入的数值属性之间具有非常大的比例差异，往往会导致机器学习算法性能表现不佳，如房屋数据里的房间总数范围大致从 6 ~ 39320，而收入中位数的范围是 0 ~ 15。（注意：目标值通常不需要缩放）

  同比例缩放所有数值属性的常用方法：

  * **最小-最大缩放**（又称归一化）——`Scikit-Learn` 中的 `MinMaxScaler` 转换器

    将值减去最小值并除以最大值和最小值的差，数据范围将缩放至 0 ~ 1。若有时候期望的范围不是 0 ~ 1，则可以调整超参数 `feature_range` 来更改最终数据范围。

    ```python
    from sklearn.preprocessing import MinMaxScaler
    
    min_max_scaler = MinMaxScaler(feature_range=(-1, 1))
    housing_num_min_max_scaled = min_max_scaler.fit_transform(housing_num)
    ```
  
  * **标准化** ——`Scikit-Learn` 中的 `StandardScaler` 转换器
  
    将值减去平均值（使得标准化后数据的均值总是零），然后除以方差（从而使得结果的分布具备单位方差）。
  
    区别：不同于归一化，标准化得到的值不会被绑定在一个特定范围（这有时会成为问题），对于每个属性/每列来说所有数据都聚集在 0 附近，方差为 1。但该方法受**异常值**的影响更小。
    
    ```python
    from sklearn.preprocessing import StandardScaler
    
    std_scaler = StandardScaler()
    housing_num_std_scaled = std_scaler.fit_transform(housing_num)
    ```
    
    验证标准化效果
    
    ```python
    # 输出标准化后数据的数值分布图 —— 均匀分布（并不一定是标准正态分布）
    fig, axs = plt.subplots(1, 2, figsize=(8, 3), sharey=True)
    housing["population"].hist(ax=axs[0], bins=50)
    housing["population"].apply(np.log).hist(ax=axs[1], bins=50) # 以 e 为底
    axs[0].set_xlabel("Population")
    axs[1].set_xlabel("Log of population")
    axs[0].set_ylabel("Number of districts")
    save_fig("long_tail_plot")
    plt.show()
    
    
    # 每个数值转换成其自身的百分位上的数值，展示所得数据的均匀分布性
    percentiles = [np.percentile(housing["median_income"], p)
                   for p in range(1, 100)]
    flattened_median_income = pd.cut(housing["median_income"],
                                     bins=[-np.inf] + percentiles + [np.inf],
                                     labels=range(1, 100 + 1))
    flattened_median_income.hist(bins=50)
    plt.xlabel("Median income percentile")
    plt.ylabel("Number of districts")
    plt.show()
    # 注：第一百分位以下的收入标注为 1，第九十九百分位以上的收入标注100。这就是为什么下面的分布范围从1到100（而不是 0 到 100）。
    ```
    
  
  特别注意：跟所有转换一样，缩放器**只能用来拟合训练集**（即其中所需的数据如最值、平均值、方差等均从训练集中统计而来），但不能从完整数据集（包括测试集）获取对应数据信息。这样才可以确保在缩放的过程中不会使用测试集的信息，从而更好地模拟实际应用中的情况。这样可以确保模型在预测时对未见过的数据有更好的泛化能力。



* **转换流水线**

  一般地，许多数据转换的步骤需要以正确的顺序来执行，通常使用`Scikit-Learn` 中的 `Pipeline`类来按顺序对数据进行各种转换。值得注意的是，在 `Pipeline()` 的参数列表中除了最后有一个是估算器之外，前面都必须是转换器（即拥有方法 `fit_transform()`），且命名不能含有双下划线。
  
  其工作原理：当调用流水线的 `fit()`方法时，会在所有转换器上按顺序依次调用 `fit_transform()`方法，并将前一个调用的输出作为参数传递给下一个调用方法中，直到传递到最终的估算器时则只会调用其 `fit()`方法。
  
  构造方式：`Pipeline` 是使用 `(key, value)` 对的列表构建的，其中`key`是步骤名称的字符串，而`value`是一个估计器对象。
  
  ```python
  # 数值属性的转换流水线
  # 写法1：
  from sklearn.pipeline import Pipeline
  
  num_pipeline = Pipeline([
      ("impute", SimpleImputer(strategy="median")),
      ("standardize", StandardScaler()),
  ])
  
  # 写法2：舍去命名
  from sklearn.pipeline import make_pipeline
  
  num_pipeline = make_pipeline(SimpleImputer(strategy="median"), StandardScaler())
  
  # 获取转换后数据并查看基本信息
  housing_num_prepared = num_pipeline.fit_transform(housing_num)
  housing_num_prepared[:2].round(2) # 展示两组数据
  
  num_pipeline.steps # 查看整个流水线的所有步骤的全部内容
  
  num_pipeline[1] # 查看第二步的内容
  
  num_pipeline[:-1] # 查看倒数第一步的内容
  
  num_pipeline.named_steps["simpleimputer"] # 获得指定某名称的转换步骤的内容
  
  num_pipeline.set_params(simpleimputer__strategy="median") # 设置某步骤函数的参数
  ```
  
  到目前为止，我们分别处理了类别列（独热编码）和数值列（上述转换流水线），实际上使用`Scikit-Learn` 中的 `ColumnTransformer`转换器可以处理**所有列**，它会将适当的转换应用于每个列。
  
  构造方式：需要一个元组列表，其中每个元组包含一个名字、一个转换器，以及一个该转换器能够应用的列名字（或索引）的列表。
  
  ```python
  # 分开处理数值列和文本列(类别列)
  num_attribs = ["longitude", "latitude", "housing_median_age", "total_rooms",
                 "total_bedrooms", "population", "households", "median_income"]
  cat_attribs = ["ocean_proximity"]
  
  # 写法1：
  from sklearn.compose import ColumnTransformer
  
  cat_pipeline = make_pipeline(
      SimpleImputer(strategy="most_frequent"),
      OneHotEncoder(handle_unknown="ignore"))
  
  preprocessing = ColumnTransformer([
      ("num", num_pipeline, num_attribs),
      ("cat", cat_pipeline, cat_attribs),
  ])
  
  # 写法2：省去命名
  from sklearn.compose import make_column_selector, make_column_transformer
  
  preprocessing = make_column_transformer(
      (num_pipeline, make_column_selector(dtype_include=np.number)),
      (cat_pipeline, make_column_selector(dtype_include=object)),
  )
  
  # 获取转换后数据
  housing_prepared = preprocessing.fit_transform(housing)
  
  # extra: 可以转换为 DataFrame 结构
  housing_prepared_fr = pd.DataFrame(
      housing_prepared,
      columns=preprocessing.get_feature_names_out(),
      index=housing.index)
  housing_prepared_fr.head(2)
  ```
  
  值得注意的是，若`cat_pipeline` 返回一个稀疏矩阵，且 `num_pipeline` 返回一个密集矩阵。对于一个`ColumnTransformer`来说，当稀疏矩阵和密集矩阵并存时，它会估算最终矩阵的密度（即单元格的非零比率），如果密度低于给定阈值（ `sprase_threshold = 0.3`）则会返回一个稀疏矩阵，否则返回一个密集矩阵。
  
  其他例子
  
  ```python
  # 自定义转换器
  def column_ratio(X):
      return X[:, [0]] / X[:, [1]]
  
  def ratio_name(function_transformer, feature_names_in):
      return ["ratio"]  # feature names out
  
  
  # 构造转换流水线
  def ratio_pipeline():
      return make_pipeline(
          SimpleImputer(strategy="median"),
          FunctionTransformer(column_ratio, feature_names_out=ratio_name),
          StandardScaler())
  
  log_pipeline = make_pipeline(
      SimpleImputer(strategy="median"),
      FunctionTransformer(np.log, feature_names_out="one-to-one"),
      StandardScaler())
  
  
  # 构造默认情况下的转换流水线
  default_num_pipeline = make_pipeline(SimpleImputer(strategy="median"),
                                       StandardScaler())
  
  
  # 基于 K-均值聚类算法 的转换器
  cluster_simil = ClusterSimilarity(n_clusters=10, gamma=1., random_state=42)
  
  
  # 应用至所有列的转换器
  preprocessing = ColumnTransformer([
          ("bedrooms", ratio_pipeline(), ["total_bedrooms", "total_rooms"]),
          ("rooms_per_house", ratio_pipeline(), ["total_rooms", "households"]),
          ("people_per_house", ratio_pipeline(), ["population", "households"]),
          ("log", log_pipeline, ["total_bedrooms", "total_rooms", "population",
                                 "households", "median_income"]),
          ("geo", cluster_simil, ["latitude", "longitude"]),
          ("cat", cat_pipeline, make_column_selector(dtype_include=object)),
      ],
      remainder=default_num_pipeline)  # one column remaining: housing_median_age
  # remainder 参数表示对剩余所有列的操作，可以传入转换器，或使用一些预设参数值如下：
  # "drop"：删除没有被指定转换操作的剩余列，默认为 "drop"
  # "passthrough"：其他列保持不变，即不进行其他操作
  
  
  housing_prepared = preprocessing.fit_transform(housing) # 应用至全部数据中
  housing_prepared.shape # 读取矩阵的长度
  preprocessing.get_feature_names_out() # 获得转换器名称与所作用的列名称
  ```



## 选择和训练模型

该步骤的目的是筛选出几个（2~5个）有效的模型，值得注意的是，对每个尝试过的模型都应该妥善保存，包括其超参数值和训练过的参数，以及交叉验证的评分和实际预测结果等，方便对比不同模型的评分以及不同模型造成的错误类型。

* 尝试多个可用模型并互相对比（一般是基于 `RMSE`的大小评估模型效果）

  ```python
  # 1.加入线性回归模型作为最后的估算器
  from sklearn.linear_model import LinearRegression
  
  lin_reg = make_pipeline(preprocessing, LinearRegression())
  lin_reg.fit(housing, housing_labels)
  
  # 预测并对比预测值和真实值
  housing_predictions = lin_reg.predict(housing)
  housing_predictions[:5].round(-2)  # -2 = rounded to the nearest hundred
  housing_labels.iloc[:5].values
  
  # 获得预测值和真实值的误差比率
  error_ratios = housing_predictions[:5].round(-2) / housing_labels.iloc[:5].values - 1
  print(", ".join([f"{100 * ratio:.1f}%" for ratio in error_ratios]))
  
  # 输出 RMSE 来评估模型
  from sklearn.metrics import mean_squared_error
  
  lin_rmse = mean_squared_error(housing_labels, housing_predictions,
                                squared=False)
  lin_rmse
  
  
  
  # 2.加入决策树算法作为最后的估算器
  from sklearn.tree import DecisionTreeRegressor
  
  tree_reg = make_pipeline(preprocessing, DecisionTreeRegressor(random_state=42))
  tree_reg.fit(housing, housing_labels)
  
  # 预测并输出决策树模型在该训练集下的RMSE
  housing_predictions = tree_reg.predict(housing)
  tree_rmse = mean_squared_error(housing_labels, housing_predictions,
                                squared=False)
  tree_rmse
  ```

  若 `RMSE`的数量级与目标值范围相仿或者更高，则说明预测效果不佳；若 `RMSE`的值对于目标值范围来说很小则可能是效果较好，也可能是过拟合（特别是 `RMSE` 为 `0`时）。

* 使用**交叉验证**来进行更好的评估

  * 在上述模型中，我们可以尝试选择表现较好的决策树模型来进行下一步评估。

    1. 使用 `train_test_split`函数将训练集分为较小的训练集和验证机，然后根据这些较小的训练集来训练模型，并对其进行评估

    2. 使用 `Scikit-Learn`的**K-折交叉验证**功能：将训练集随机分割成 `cv`个不同的子集（每个子集成为一个折叠），然后对模型进行 `cv`次**训练和评估**（每次选出其中 1 个折叠进行评估，其余全部用来训练模型），最终会得到包含 `cv`次评估分数的数组。

       注：该功能更倾向于使用效用函数（越大越好）而不是成本函数，故计算分数的函数实质是**负的MSE**，所以通常在其结果前添加负号以变成非负数。

       ```python
       # K-折交叉验证
       from sklearn.model_selection import cross_val_score
       
       tree_rmses = -cross_val_score(tree_reg, housing, housing_labels,
                                     scoring="neg_root_mean_squared_error", cv=10) # 分为 10 个折叠
       
       pd.Series(tree_rmses).describe() # 获取预测结果的基本数据
       
       
       
       # 线性回归模型
       lin_rmses = -cross_val_score(lin_reg, housing, housing_labels,
                                     scoring="neg_root_mean_squared_error", cv=10)
       pd.Series(lin_rmses).describe() # 获取预测结果的基本数据
       # 结果发现，决策树模型确实过拟合了，以至于比线性回归模型的表现还差
       
       
       
       # 随机森林算法
       from sklearn.ensemble import RandomForestRegressor
       
       forest_reg = make_pipeline(preprocessing,
                                  RandomForestRegressor(random_state=42))
       forest_rmses = -cross_val_score(forest_reg, housing, housing_labels,
                                       scoring="neg_root_mean_squared_error", cv=10)
       
       pd.Series(forest_rmses).describe()
       # 结果发现，随机森林模型的表现较好，但仍很可能过拟合训练集（发现训练误差远低于验证误差，这通常意味着模型已经过拟合了训练集。另一种可能的解释可能是，训练数据和验证数据之间不匹配，但这里的情况并非如此，因为两者都来自同一个数据集，我们已将其混合并分为两部分）
       
       # 将使用交叉验证测量的RMSE（“验证误差”）与在训练集上测量的RMSE[“训练误差”]进行比较
       # 以下是在训练集上预测目标值并测量RMSE
       forest_reg.fit(housing, housing_labels)
       housing_predictions = forest_reg.predict(housing)
       forest_rmse = mean_squared_error(housing_labels, housing_predictions,
                                        squared=False)
       forest_rmse
       ```



## 微调模型

此时我们已经拥有了一个有效模型的候选列表，现需要对它们进行微调，也就是调节超参数的值，以使得该模型趋于它自身的最佳表现。

一种微调方法是手动调整超参数，这显然耗时费力，以下是几种更好的调节超参数的方法（基于 `Scikit-Learn`中的类或方法）：

* **网格搜索（Grid Search）**

   `GridSearchCV`类接受一个参数字典或参数列表作为输入，其中包含了待调优的参数及其可能的取值范围。它会遍历所有可能的参数组合，并对每一组参数进行**交叉验证**来评估模型的性能。其参数列表如下：

  * `estimator`: 指定要使用的机器学习模型，例如分类器或回归器。
  * `param_grid`: 参数字典或参数列表，包含待调优的参数及其可能的取值范围。
  * `scoring`: 评估模型性能的指标，接收字符串（如`'accuracy'`、`'neg_root_mean_squared_error'`等）或可调用对象（如`sklearn.metrics.accuracy_score`）。默认为`'None'`，使用`estimator`的误差估计函数。当然，也可以自定义度量函数。
  * `cv`: 交叉验证的折数，默认为 `None`则会使用 3-折交叉验证。
  * `refit`: 是否在搜索结束后使用最佳参数重新拟合整个数据集，默认为 `True`即会重新拟合。
  * `iid`：默认为`True`，即各个样本`fold`概率分布一致，误差估计为所有样本之和，而非各个fold的平均。
  * `verbose`：（一般不管）日志冗长度，指定输出的详细程度，表示输出每个参数组合的详细信息，预设 0：不输出训练过程，1：偶尔输出，>1：对每个子模型都输出。
  * `n_jobs`：（一般不管）指定并行运行的作业数，表示使用所有可用的CPU核心个数，预设 -1：跟CPU核数一致, 1：默认值。
  * ······

  注：当你不知道超参数该尝试哪些值时，较为简单的方法是尝试 10 的连续幂次（或者若想要得到更细粒度的搜索，则可以使用更小的幂底数）。

  ```python
  # 以下是利用网格搜索来寻找随机森林模型的超参数值的最佳组合
  from sklearn.model_selection import GridSearchCV
  
  full_pipeline = Pipeline([
      ("preprocessing", preprocessing),
      ("random_forest", RandomForestRegressor(random_state=42)),
  ])
  param_grid = [
      {'preprocessing__geo__n_clusters': [5, 8, 10],
       'random_forest__max_features': [4, 6, 8]},
      {'preprocessing__geo__n_clusters': [10, 15],
       'random_forest__max_features': [6, 8, 10]},
  ] # 一共进行两次网格搜索，第一次进行 3*3 次组合与评估，第二次进行 2*3 次组合与评估
  
  grid_search = GridSearchCV(full_pipeline, param_grid, cv=3,
                             scoring='neg_root_mean_squared_error')
  grid_search.fit(housing, housing_labels)
  
  # 你可以以此查询可调节超参数的列表（前1000个）
  print(str(full_pipeline.get_params().keys())[:1000] + "...")
  
  # 输出网格搜索得到的最佳值
  grid_search.best_params_
  
  # 返回网格搜索过程中评分最高的估算器对象
  grid_search.best_estimator_
  
  # 输出网格搜索过程中各组合的评估分数
  cv_res = pd.DataFrame(grid_search.cv_results_)
  cv_res.sort_values(by="mean_test_score", ascending=False, inplace=True) # 将以各组合的分数排序
  
  # extra code – these few lines of code just make the DataFrame look nicer
  cv_res = cv_res[["param_preprocessing__geo__n_clusters",
                   "param_random_forest__max_features", "split0_test_score",
                   "split1_test_score", "split2_test_score", "mean_test_score"]]
  score_cols = ["split0", "split1", "split2", "mean_test_rmse"]
  cv_res.columns = ["n_clusters", "max_features"] + score_cols
  cv_res[score_cols] = -cv_res[score_cols].round().astype(np.int64)
  
  cv_res.head()
  ```

  显然，网格搜索是一种暴力枚举，若组合数量较大则会使得成本变高。

* **随机搜索（Randomized Search）**

  适用于超参数的搜索范围较大时。`RandomizedSearchCV`类接收一个字典来指定超参数以及指定了数据范围的随机器，并通过参数 `n_iter`指定迭代次数（即总随机次数），每次迭代都会从每个超参数中按其指定数据范围来取一个随机值，并通过**交叉验证**进行评估。其参数列表与网格搜索相仿，不同的有：

  * `param_distributions`：接受一个字典，每个配对组成为 `key`：超参数名称，`value`：指定了数据范围的随机器（运行时返回一个整数，每次迭代都会重新调用随机器）。
  * `n_iter`：指定总迭代次数。
  * `random_state`：随机种子，一般取同一个值（如 42）以保证可以复现实验结果。

  ```python
  # 以下是利用随机搜索来寻找随机森林模型的超参数值的最佳组合
  from sklearn.experimental import enable_halving_search_cv
  from sklearn.model_selection import HalvingRandomSearchCV
  from sklearn.model_selection import RandomizedSearchCV
  from scipy.stats import randint
  
  param_distribs = {'preprocessing__geo__n_clusters': randint(low=3, high=50),
                    'random_forest__max_features': randint(low=2, high=20)}
  
  rnd_search = RandomizedSearchCV(
      full_pipeline, param_distributions=param_distribs, n_iter=10, cv=3,
      scoring='neg_root_mean_squared_error', random_state=42)
  
  rnd_search.fit(housing, housing_labels)
  
  # extra code – displays the random search results
  cv_res = pd.DataFrame(rnd_search.cv_results_)
  cv_res.sort_values(by="mean_test_score", ascending=False, inplace=True)
  cv_res = cv_res[["param_preprocessing__geo__n_clusters",
                   "param_random_forest__max_features", "split0_test_score",
                   "split1_test_score", "split2_test_score", "mean_test_score"]]
  cv_res.columns = ["n_clusters", "max_features"] + score_cols
  cv_res[score_cols] = -cv_res[score_cols].round().astype(np.int64)
  cv_res.head()
  ```

调整超参数的值后，还可以对**最优模型**（可能有多个）进一步调节与评估，以下是几种调节与评估方法：

* **集成方法**

  将表现最优的若干个模型组合起来，通常组合模型的表现会比单一模型的表现更好（就像随机森林比其所依赖的任何单个决策树模型表现更好一样），特别是当若干单一模型会产生不同类型误差时更是如此。

* **分析最佳模型及其误差**

  通过检查最佳模型以获取洞见，当在进行准确预测时，可以输出超参数调节至最佳后模型的每个属性的相对重要程度

  ```python
  final_model = rnd_search.best_estimator_  # includes preprocessing
  feature_importances = final_model["random_forest"].feature_importances_
  feature_importances.round(2)
  
  # 将属性名称与其对应相对重要程度值一起显示
  sorted(zip(feature_importances,
             final_model["preprocessing"].get_feature_names_out()),
             reverse=True)
  ```

  此后，你可以尝试删除一些不太有用的特征。然后，还应该查看一下系统产生的具体错误，了解其产生原因并解决（通过添加额外的特征、删除没有信息的特征、清除异常值等）。

* **通过测试集评估系统**

  从**测试集**中获取预测器和标签，再使用完整的数据流水线进行数据转换（此时调用的是 `transform()`，而不是 `fit_transform()`），最后获取最终评估结果。

  ```python
  X_test = strat_test_set.drop("median_house_value", axis=1) # 分离出标签的预测器
  y_test = strat_test_set["median_house_value"].copy() # 标签
  
  # 预测并输出预测误差
  final_predictions = final_model.predict(X_test)
  final_rmse = mean_squared_error(y_test, final_predictions, squared=False)
  print(final_rmse)
  
  # 需要知道泛化误差估计的准确度，可以计算泛化误差的 95% 置信区间
  from scipy import stats
  
  confidence = 0.95
  squared_errors = (final_predictions - y_test) ** 2
  np.sqrt(stats.t.interval(confidence, len(squared_errors) - 1,
                           loc=squared_errors.mean(),
                           scale=stats.sem(squared_errors)))
  
  # 也可以手动计算间隔，结果相近
  m = len(squared_errors)
  mean = squared_errors.mean()
  tscore = stats.t.ppf((1 + confidence) / 2, df=m - 1)
  tmargin = tscore * squared_errors.std(ddof=1) / np.sqrt(m)
  np.sqrt(mean - tmargin), np.sqrt(mean + tmargin)
  
  # 或者，我们可以使用 z-score 而不是 t-score。由于测试集不太小，所以不会有太大的区别
  zscore = stats.norm.ppf((1 + confidence) / 2)
  zmargin = zscore * squared_errors.std(ddof=1) / np.sqrt(m)
  np.sqrt(mean - zmargin), np.sqrt(mean + zmargin)
  ```

  如果之前进行过大量的超参数调整，这时的评估结果通常会略逊于之前使用交叉验证时的表现结果（因为经过不断调整，系统在验证数据上终于表现良好，但在未知数据集或测试集上可能达不到同样的效果）。当出现上述情况时，**一定不能继续调整超参数**，因为这样也只是让测试集的结果变得好看而并非实质提升对未知数据的泛化效果。



## 启动、监控和维护系统

......



# 分类

## MNIST

MNIST数据集是一组手写得70000个数字的图片，并且每张图片都用其代表的数字标记，只要是分类算法，都可以试着在其上执行，相当于机器学习领域的 Hello World。而 `Scikit-Learn` 提供了许多功能帮助你下载流行的数据集，包括MNIST数据集。

`Scikit-Learn` 得到的数据集通常具有类似的字典结构，包括以下 `key` 值：

* `"DESCR"`：描述数据集
* `"data"`：包含一个数据，每个实例为一行，每个特征为一列
* `"target"`：包含一个带有标记的数组

```python
# 获取MNIST数据集
from sklearn.datasets import fetch_openml

mnist = fetch_openml('mnist_784', as_frame=False)

print(mnist.DESCR)

# 获得MNIST数据集的所有键
mnist.keys()  # we only use "data" and "target" in this notebook

# 分别获得数据集和标记并了解各自的数组大小
X, y = mnist.data, mnist.target 
X
X.shape
y
y.shape

# 将图片重组为 28 * 28 大小的数组(或者说方阵)，即含有 784 个特征，每个特征代表一个像素点的强度（0~255）
import matplotlib.pyplot as plt

def plot_digit(image_data):
    image = image_data.reshape(28, 28)
    plt.imshow(image, cmap="binary")
    plt.axis("off")

some_digit = X[0]
plot_digit(some_digit)
save_fig("some_digit_plot")  # extra code
plt.show()

# 比较重组后图片和标签值，发现图片特征仍是比较清晰且可辨认的，故采用该大小
y[0] 

# 取前 60000 个样本作为训练集，其余（约10000个）作为测试集
X_train, X_test, y_train, y_test = X[:60000], X[60000:], y[:60000], y[60000:] 


```

其中，`numpy`中的 `reshape`方法的使用方式：

1. `arr.reshape(a, b)  # 改变维度为 a 行、b 列`
2. `arr.reshape(m, -1) # 改变维度为 m 行、d 列 （-1表示列数自动计算，d = a * b / m ）`
3. `arr.reshape(-1, m) # 改变维度为 d 行、m 列 （-1表示行数自动计算，d = a * b / m ）`



## 训练二元分类器

二元分类器，顾名思义就是对数据在**两个类**中区分，如对于所有数字的图片只尝试识别一个数字 5，其余都是非 5 数字

```python
# 训练一个识别数字 5 的二元分类器
# 构造目标向量，为 5 则标记为 True，非 5 则标记为 False
y_train_5 = (y_train == '5')
y_test_5 = (y_test == '5')

# 训练随机梯度下降（SGD）分类器
from sklearn.linear_model import SGDClassifier

sgd_clf = SGDClassifier(random_state=42) # 设置参数 random_state 方便复现
sgd_clf.fit(X_train, y_train_5)

sgd_clf.predict([some_digit]) # 预测上文某个数据
```



## 性能测量

评估一个分类器的几种方法

* **交叉验证**

  ```python
  # 法一：利用 Scikit-Learn 库函数
  from sklearn.model_selection import cross_val_score
  
  cross_val_score(sgd_clf, X_train, y_train_5, cv=3, scoring="accuracy")
  
  # 法二：手动实现交叉验证
  from sklearn.model_selection import StratifiedKFold
  from sklearn.base import clone
  
  skfolds = StratifiedKFold(n_splits=3)  # add shuffle=True if the dataset is not
                                         # already shuffled
  for train_index, test_index in skfolds.split(X_train, y_train_5):
      clone_clf = clone(sgd_clf)
      X_train_folds = X_train[train_index]
      y_train_folds = y_train_5[train_index]
      X_test_fold = X_train[test_index]
      y_test_fold = y_train_5[test_index]
  
      clone_clf.fit(X_train_folds, y_train_folds)
      y_pred = clone_clf.predict(X_test_fold)
      n_correct = sum(y_pred == y_test_fold)
      print(n_correct / len(y_pred))
  ```

  

* 
