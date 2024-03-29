# Python

---

# 零、基础知识

---

## 包和模块

### 概念

1. 模块(module)：简单来说就是 .py 文件，其中包含了一组相关的函数、类、变量和常量。通常情况下，每个模块都会定义一组相关的功能，以便在其他Python程序中重复使用。

2. 包(package)：包是一个包含多个模块或者多个子包的有层次的文件目录结构。通常情况下，包中的每个模块都会定义一组相关的功能，以便在其他Python程序中重复使用，且该目录下一定包含`__init__.py`文件。

   创建包时：创建一个文件夹, 文件夹内务必创建一个`__init__.py`文件(其实python3.3版本之后不必创建, 但是为了代码兼容, 以及做一些其他包处理操作, 目前还是建议创建)

3. 库：指完成一定功能的代码集合，在python中的形式是模块和包。如：

   - NumPy：用于数值计算和科学计算的库
   - Pandas：用于数据分析和数据处理的库
   - Flask：用于Web开发的轻量级框架
   - Django：用于Web开发的全功能框架
   - TensorFlow：用于机器学习的库
   - Matplotlib：用于数据可视化的库

### 分类

1. 标准包/模块：标准库包是Python语言本身提供的包，无需额外安装即可使用。这些包包括`os`、`sys`、`re`、`math`等，它们提供了许多常用的功能。
2. 第三方包/模块：第三方包是由第三方开发人员或团队开发的包，提供了各种不同的功能。这些包需要通过`pip`或其他包管理器进行安装，例如`numpy`、`requests`、`pandas`等。
3. 自定义包/模块

### 导入

1. `import M`: 使用`import`语句可以导入一个模块, 如果是某个包里面的模块, 则可以通过点语法来定位, 需要注意的是每次调用包时, 都会先执行包里面的`__init__.py` 文件

2. `import M1, M2` : 导入多个模块, 可以用逗号隔开

3. `import M as ...` : 使用import...as语句可以给导入的模块或函数起一个别名, 可以简化资源访问前缀, 增加程序的扩展性

4. `from ... import ...[as ...]`: 从一个模块中导入**指定**的函数、类或变量; 需要明确的是只能从大的地方找小的东西: 包 > 模块 > 模块资源, 而且注意面向关系, 即包只能看到模块, 模块只能看到模块资源

   ```python
   # 只能从包到模块, 模块到模块资源
   from p1 import Tool1 as t1, Tool2 as t2      # 多模块
   from p1 import Tool1, Tool2
   from p1.sub_p import sub_tool                # 多层级
   ```

5. `from ... import * [as ...]` : 导入一个模块或者包中的**所有非下划线开头**的公共函数、类和变量, 在使用时我们需要小心, 因为我们无法预知会导入哪些内容到当前位置, 容易产生变量名冲突

   ```python
   # 也可以通过在模块中重写__all__列表, 来选择我们所认为的需要从模块中导入的资源, 或者从包中需要导入的模块; 注意列表中每个元素都是字符串, 即引号包含变量名称或者文件名称
   __all__ = ["num1", ... ]
   ```



---

## 输入与输出

**输入**

`input(str)` 语句以**字符串**的形式接收用户输入，并在**输入前**先输出字符串 `str` 作为提示信息。该语句返回用户输入的字符串。

可以利用强制类型转换如 `int(input())` 来转换所接收数据的类型，如果输入的数据不适配待转换的类型则会报错。

```python
str = input("请输入你的姓名:") 
print(type(str))
age = int(input("请输入你的年龄:"))
print(type(age))
a = input("请输入一个字符串:") 
print(a)
a = input("请输入一个字符：\n") 
print(a)
 
 
结果：
请输入你的姓名:卿云
<class 'str'>
 
请输入你的年龄:19
<class 'int'>
 
请输入一个字符串:我喜欢曹同学
我喜欢曹同学
 
请输入一个字符：
z
z
```

**输出**

* 标准输出：

  ![image-20230427204639320](C:\Users\asus\AppData\Roaming\Typora\typora-user-images\image-20230427204639320.png)

  ```python
  print('abc', 123)
  输出：abc 123
  
  print('abc', 123, sep=',')
  输出：abc,123
  
  print('abc', end = '')
  print(123)
  输出：abc123
  
  # 输出 ASCII 码值
  print(ord('c')) # 99
  ```

  

* 格式化输出：`print("%d" % num) 或 print("%s %d" % (str, num)) 等`，类似 C\C++ 语言的格式化输入

  <img src="D:\MarkdownText\image_python\7.png" alt="7" style="zoom:67%;" />

* 格式化函数 `str.format()`

  它是字符串类型数据的成员方法，用于将字符串自动格式化（返回格式化写法的字符串），根据符号 `{}` 内参数分为：

  写法1：`print("{} {}".format(x1, x2))`，**按顺序**对应，第一个`{}`对应第一个接收变量 `x1`，以此类推；

  写法2：`print("{n1} {n2}".format(n1 = x1, n2 = x2)) 或 print("{n1} {n2}".format(n2 = x2, n1 = x1))`，此时 `{NAME}`内给定一个**形式变量名**，可以将待输出值赋给该形式变量，故**无需**按顺序填写待输出变量。

  写法3：`print("{0} {1}".format(x1, x2))`，此时 `{Index}` 内为从参数列表中引用的变量的下标（从 0 开始编号），故编号 0 对应参数列表中第一个变量 `x1`，以此类推。

  ```python
  x1 = 1
  x2 = 2
  
  三种效果等同的写法：
  print("{} {}".format(x1, x2))				//1
  print("{n1} {n2}".format(n2 = x2, n1 = x1))	 //2
  print("{0} {1}".format(x1, x2))				//3
  都输出：1 2
  
  print("{1} {0}".format(x1, x2))
  输出：2 1
  ```

* `f-strings`格式化输出

  在字符串前添加前缀标识符 `f`，则可在其中使用 `{变量}` 操作符接收变量

  ```python
  name = '秦 sir'
  age = 20
  sex = '男'
  res = f"我的名字叫：{name.upper()}，我今年{age + 1}岁，我是{sex}生"
  print(res)
  
  输出：我的名字叫：秦 SIR，我今年21岁，我是男生
  ```

  

---

# 一、STL

----

## 前言

字符串、列表、元组 都属于python中的内置序列类型，都支持**索引取值**和**切片操作**，当**索引为负数**时则是指向右端，从右端开始计算。

切片：`[start : end : step]`，其中步长 step 默认为 1





## 字符串

---

> 不同于其他语言的字符串，Python 中的字符串是**不可变**的序列数据类型，支持**索引取值**和**切片操作**，但无法索引赋值。

<img src="D:\MarkdownText\image_python\1.png" alt="1" style="zoom: 50%;" />

```python
t = "abcdefghijklmnopqrstuvwxyz"
t2 = "    12312e1   "
t3 = "ab eda wrq w dqw dqw qwqw q"
t4 = "ADADaidAOIDJaiad wJDAJ A"

print("{}".format(t[1:10]))
print("{}".format(t[2:-1]))
print("{}".format(t[3:].capitalize())) #首字母变大写
print("{}".format(t[3:].lower())) #全部变小写
print("{}".format(t[3:].upper())) #全部变大写
print("{}".format(t3.title())) #每个单词首字母变大写
print("{}".format(t4.swapcase())) #大写变小写，小写变大写
print("{}".format(t2.strip())) #去除首尾的空格
print("{}".format(t2.lstrip())) #去除前部分空格 同理去除后部分空格 t2.rstrip()
print("res = {}".format(t2.find("123"))) #返回下标
print("res = {}".format(t2.find("12322"))) #找不到返回 -1
print("res = {}".format(t2.endswith(" "))) #是否以什么结尾
print("res = {}".format(t2.startswith(" "))) #是否以什么起头
print("cnt = {}".format(t2.count("1"))) #统计出现次数
print("replace = {}".format(t2.replace("1", "x", 2))) #将 old 替换为 new，仅替换前 2 个
print("replace = {}".format(t2.replace("1", "x"))) #count 为空表示全部替换
print("join = {}".format("-".join(t3))) #取出 text 所有单词并用 ··· 来连接

```









---

## 列表

> 一种**可修改可遍历**的有序数据集合，其数据项可以变化，但内存地址不会变。它可以**同时存储任意不同类型的数据**。

用 `[]` 来创建一个列表

<img src="D:\MarkdownText\image_python\2.png" alt="2" style="zoom: 50%;" />



```python
lis = [1, 2, 3, 4, 5, 6, 1, "!@3", 1.02, "x", [2, 3, "?"], True]
lis[0] = "222" #支持不同类型的数据的修改覆盖

print("len = {}".format(len(lis))) #实际上 len() 可以获取三种序列数据类型的长度
del lis[0] #序列操作——删除元素
print(lis)
del lis[2 : 5] #批量删除
print(lis)
lis.remove("!@3") #删除指定元素值为 x 的元素
print(lis)
lis.pop(0) #删除指定下标 i 的元素
print(lis)
print("ind = {}".format(lis.index('x'))) #查找指定元素值的元素下标值

print(lis[2])
print(lis[2 : 3]) #下标为 [2, 3) 
print(lis[:: -1]) #负数步长从右开始逆序输出
print(lis * 3) #输出 n 次数据

lis.append("das")
print(lis)
lis.append(["加入一个列表元素", 12, 213])
print(lis)
lis.insert(2, "插入") # 在下标为 2 的元素前插入数据
print(lis)

lis1 = list(range(10)) # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(lis1)
lis.extend("lis1") #拓展，批量添加
print(lis)

#append 和 extend 的差别
lis.append("1234") #仅将数据 "1234" 一整个添加到 lis 中
print(lis)
lis.extend("1234") #将序列"1234"每个元素全部批量添加到 lis 中
print(lis)

lis.reverse() #翻转列表
print(lis)

lis2 = [21312, 32412,312,31,12,12,12,1 ,1]
lis2.sort() #排序(仅对列表只有同类型数据时有用)
print(lis2)

```



---

## 元组

> 是一种**不可修改**的序列，创建后不可变，可以**同时存储任意不同类型**的数据。特殊地。**当元组中只有一个元素时需要加上后缀逗号**，防止解释器误解成整型。支持**索引取值**和**切片操作**。

用 `()` 来创建一个元组

注意：元组元素的不可修改性是指**元素内存地址的不可修改**，若元组元素为**常量**（基本数据常量、字符串常量等）则彻底不可修改（因为常量的内存地址固定，一旦改变就会改变内存地址）；若是某种**可修改序列类型**（如列表、字典），则可以修改它们的内部值，而不会影响列表或字典本身的内存地址，那么可以说元组元素的内存地址没有被修改。

```python
lis = ["123", 1]
dic = {"age" : "18"}
tuple1 = (100, "@", lis, dic)
# 元组中列表可修改
tuple1[2][1] = "XX"
print(tuple1)
# 元组中字典可修改
tuple1[3]["age"] = "30"
print(tuple1)
```



```python
tupleA = (1, )
tuple1 = (1, "asdc", 2.121, [2, 3, 4])

print(tuple1[0])
print(tuple1[2 :])
print(tuple1[-2 : -1]) # 只有单个元素时输出 (2.121,) 这个逗号和括号共同说明他是个元组
print(tuple1)
#特殊地，元组内的列表可以修改
tuple1[3][1] = "Y"
print(tuple1)
print("address = {}".format(id(tuple1))) # id() 输出数据内存地址
#for it in tuple1:
#    print(it, end = ' ')
tupleB = tuple(range(10))
print(tupleB)
print("cnt = {}".format(tupleB.count(8)))
```



---

## 字典

> 同 c/c++ 的map，它以 `{'key' : 'value'}` 键值对的形式创建，字典的键（key）不可重复，且键（key）只能是**不可变数据类型**（如数字、字符串、元组）。它是一种**无序**的键值集合，为内置的高级数据结构，优点是**效率高**。

如果存在重复的键（key）的添加，则后添加会覆盖前面的。

<img src="D:\MarkdownText\image_python\3.png" alt="3" style="zoom: 50%;" />

```python
dict1 = {"name" : "limab", "age" : 30, "position" : "fuck"}

print(dict1)
print(len(dict1)) #键值对个数
print(dict1["name"]) #键值查找
dict1["name"] = "sb" #键值修改
print(dict1["name"])
print(dict1.keys()) #获取所有的键
print(dict1.values()) #获取所有的值
print(dict1.items()) #获取所有的键值对
dict1.update({"new" : "new"}) #添加或修改键值对
dict1.update({"age" : 18}) 
# 与 dict1["new"] = "new" 同理
print(dict1)

#通过指定 键 来删除键值对
del dict1["new"]
dict1.pop("position")
print(dict1)

dict1["n1"] = "12"
dict1["n2"] = "123"
dict1["n3"] = "123123"
dict1["age"] = "30"

# 排序前提：同类型
# 按照 key 排序
print(sorted(dict1.items(), key = lambda d : d[0]))
# 按照 val 排序
print(sorted(dict1.items(), key = lambda d : d[1]))
#这里的 key = ??? 的 key 是指排序依据，右值必须是一个函数，而 lambda 提供匿名函数
# lambda 参数名称:表达式  创建一个匿名函数

for key, val in dict1.items():
    print("key : val = {} : {}".format(key, val))
    pass
```



---

## 集合

> 一种内置的**可变**数据类型，用于存储一个**无序且不重复**的元素集合。性质类似于字典但是**有键无值**，同样**不支持**索引取值和切片操作。

创建方式：

```python
set0 = set() # 创建空集合

# 第一种方式
set1 = {"1", "232"}

# 第二种方式 - 使用列表初始化集合
lis = [1, 23, 231, 23, 23]
set2 = set(lis)
print(set2) # {1, 231, 23}
```

操作函数：

- 添加元素：使用 `add(元素值)` 方法添加元素到集合。
- 删除元素：使用 `remove(元素值)` 或 `discard(元素值)` 方法从集合中删除指定的元素。
- 取出元素：使用 `pop()`方法从集合中随机取出某个数据（返回该数据）并**同时删除**。
- 集合合并：使用 `set1.update(set2)` 将 `set2` 合并到 `set1` 上，即直接改变操作对象 `set1`。
- 清空集合：使用 `clear()` 方法清除集合中所有元素。
- 获取集合长度：使用 `len()` 方法获取集合中元素的数量。
- 检查元素是否存在：使用 `in` 关键字检查集合中是否存在指定的元素。
- 集合运算：交集、并集、差集等常见集合运算可以通过 `&   |   -` 等运算符实现，也可以使用其自带的函数。

```python
lis = [1, 23, 231, 23, 23]
set1 = set(lis)
set2 = set([1, 23, 123123])
# 差集操作
print(set1.difference(set2)) 
print(set1 - set2)
# 输出：{231}

# 交集操作
print(set1.intersection(set2))
print(set1 & set2)
# 输出：{1, 23}

# 并集操作
print(set1.union(set2))
print(set1 | set2)
# 输出：{1, 123123, 23, 231}
```





---

## STL公有方法

<img src="D:\MarkdownText\image_python\4.png" alt="4" style="zoom:67%;" />

```python
str1 = "abcde"
lis1 = [1, "@a$", list(range(20, 30))]
tuple1 = (3, "A")
dict1 = {"age" : "18", "name" : "Fuck"}

# 合并操作 +
str2 = "XXSACX"
tuple2 = ("B", 4)
lis2 = list(range(100, 110))
print(str1 + str2)
print(tuple1 + tuple2)
print(lis1 + lis2)

# 复制操作 *
# 实质就是对自身进行 n 次合并操作
print(str1 * 3)
print(tuple1 * 3)
print(lis1 * 3)

# 判断元素是否存在于序列中 in
# 对于字典，键 和 值都可以查找存在性
print('a' in str1)
print(1 in lis1)
print("a" in tuple1)
print("age" in dict1)
print("18" in dict1)
```



---

# 二、函数

---

## 可变参数

Python 中有两个可变参数： 可变参数`*args` 和 关键字可变参数`**kwargs`。

> `*args`（即 `*name`）表示任何多个无名参数，它是一个**元组**。当传入的参数个数未知，且不需要知道参数名称时，用来发送一个**非键值对**的**可变数量**的参数列表给一个函数。

> `**kwargs`（即`**name`）表示关键字参数，它是一个**字典**。当传入的参数个数未知，但需要知道参数的名称时，以**键值对**的形式传入**参数名称**和参数值。

```python
#缺省参数的默认值
def sum(a = 10, b = 10):
    print("sum = {}".format(a + b))
    pass

# 可变参数 ：*args 和 **kwargs （实际上名字可以不是 args 或 kwargs，这是用户自定义的，重点是前面的星号）
# args = arguments 参数
# *args：表示任何多个无名参数，它是一个元组。当传入的参数个数未知，且不需要知道参数名称时，用来发送一个非键值对的可变数量的参数列表给一个函数。
# **kwargs：表示关键字参数，它是一个字典。当传入的参数个数未知，但需要知道参数的名称时，以键值对的形式传入参数名称和参数值。
# 注意：同时使用 *args 和 **kwargs 时，在参数列表中必须满足 *args 在 **kwargs 前，否则报错
def Pri1(*args):
    print(args)
    pass

def Pri2(**kwargs):
    print(kwargs)
    pass

def Pri3(*args, **kwargs):
    print(args)
    print(kwargs)
    pass


sum()

Pri1(1, 2, 2, 32,2 ,3, "!@312", [123,213,"@3"], (True, "IKJOI"), "---")

# 两种传递方式
dic1 = {"a" : 10, "b" : "ASDFAW"}
Pri2(**dic1) #给字典变量名前加 "**"
Pri2(a = 10, b = "ASDFAW") #类似键值对

Pri3(1, 3, 45, 100) #此时 **kwargs 为空
Pri3(name = "sb", age = 18) #此时 *args 为空
Pri3(1, 3, 45, 100, name = "sb", age = 18)
```



---

## Python内置函数

<img src="D:\MarkdownText\image_python\5.png" alt="5" style="zoom: 50%;" />

[(41条消息) 【Python学习】常用函数(更新中……)_python常用函数大全_用余生去守护的博客-CSDN博客](https://blog.csdn.net/qq_45365214/article/details/125327463?ops_request_misc=&request_id=&biz_id=102&utm_term=python 常用数学函数&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-125327463.142^v86^insert_down38v5,239^v2^insert_chatgpt&spm=1018.2226.3001.4187)

其中：

<img src="D:\MarkdownText\image_python\6.png" alt="6" style="zoom: 50%;" />

* `all(iterable)`

   判断可迭代对象中所有元素是否都为 `True`，是则返回 `True`，否则返回 `False`
  除了 0、空、False 外都算 `True`，也可以理解为数据强转为 bool 类型后的值。



* `any(iterable)` 

  判断可迭代对象中所有元素是否至少一个为 True，是则返回 True，否则返回 False



* `sorted(iterable, *cmp = None, key = None, reverse = False)`
  * `iterable`：要排序的可迭代对象，例如列表、元组、字符串等。
  * `key`：可选参数，用于指定一个函数，其作用是**将每个元素转换为一个值**进行排序。如果未指定，则默认使用**元素本身**进行比较。
  * `reverse`：可选参数，用于指定是否按照**逆序**（降序）排序，默认为 `False`，即升序排序。
  * `*cmp`: 指定自定义比较函数，但在 python 3 中**已被废除**，现在都是使用 key 参数完成自定义操作。



* `range()` 

  创建一个范围在 `[start, stop)` 的以步长 `step` 的方式连续的**整数列表**



* `zip(iterable1, iterable2, ..., )` 

  作用：将两个或多个可迭代对象打包成一个**元组列表**，并返回这个列表。

  注意：每个元组中的元素都来自于输入对象的相同位置。如果输入对象的长度不同，那么 `zip()` 将以**最短的对象的长度**为准来截取其余对象该长度的部分。

  返回值：`zip()` 返回的是一个**迭代器对象**，该迭代器生成元组列表，每个元组由输入可迭代对象中相同位置的元素组成。因此可以使用它来进行迭代或转换成需要的其他数据类型（例如列表、字典等）。

  * `*zip()` 进行**解包**可以将元组列表分解成单独的**个体元组**
  * 对**可迭代对象**进行解包。解包后，可迭代对象中的每个元素都会被拆分成**单独的值**，并**按顺序**作为单独的位置参数传递给函数。



* `enumerate(iterable, start = 0)`

  作用：返回一个由元素和其对应索引组成的**迭代对象**。参数 `iterable` 是要枚举的可迭代对象，`start` 是可选参数，表示起始索引，默认为 0。返回的枚举对象的每个元素都是一个**元组**，包含一个计数（**从 start 开始计数**）和相应的值。
  
  应用：一般用在需要元素对应下标值的 for 循环中。





``` python
l1 = [1, 2, 3]
l2 = [1, 2, 0]

# 1. all()
all(l1) == True
all(l2) == False


# 2. any() 
any(l1) == any(l2) == True
any([0, 0, 0, "", False]) == False


# 3. sorted()
# sort() 是应用在 list 上的成员方法，而 sorted() 用于对可迭代对象进行排序。它返回一个新的已排序的列表，而不会改变原始的可迭代对象。
print(sorted([2, 34, 3, 231, 12, 323452 ,253, 345,657856,8,65,5]))
print(sorted([2, 34, 3, 231, 12, 323452 ,253, 345,657856,8,65,5], reverse = True))
print(sorted(['a', 'C', 'g', 'F', 'Z', 'k', 'l', 'D', 'b', 'n', 'U'], key = str.lower)) # 字符串无关大小写进行排序
print(sorted(['a', 'C', 'g', 'F', 'Z', 'k', 'l', 'D', 'b', 'n', 'U'], key = str.upper)) # 字符串无关大小写进行排序
# str.lower\upper 返回字符串变成小写\大写后的字符串，意思是 sorted 按照 key 来排，就全是小写或大写，即无关大小写

lis = [1, 323, 23,13, 1223,12,321,235, 56]
lis.sort() # sort() 直接修改原始对象，无返回值
print(lis)
lis.reverse() # 无返回值，将列表反向
print(lis)


# 4. range()
lis = range(start, stop, step = 1)


# 5. zip()
print(list(zip([1, 2, 3], ['a', 'b', 'c'], ["abc", "bcd", "cdf"]))) 
#[(1, 'a', 'abc'), (2, 'b', 'bcd'), (3, 'c', 'cdf')]

# *zip() 进行解包可以将元组序列分解成单独的个体序列
print(*zip([1, 2, 3], ['a', 'b', 'c'], ["abc", "bcd", "cdf"])) 
#(1, 'a', 'abc') (2, 'b', 'bcd') (3, 'c', 'cdf')
# 对可迭代对象进行解包
# 解包后，可迭代对象中的每个元素都会被拆分成单独的值，并按顺序作为单独的位置参数传递给函数
print(*zip(*["abc", "bcd", "cdf"])) # ('a', 'b', 'c') ('b', 'c', 'd') ('c', 'd', 'f')


# 6. enumerate(iterable, start = 0)
lis = [1, "213!@#", 1.23923, 'c']
print(list(enumerate(lis))) # [(0, 1), (1, '213!@#'), (2, 1.23923), (3, 'c')]
for index, val in enumerate(lis):
    print("id = {}, val = {}".format(index, val))
# id = 0, val = 1
# id = 1, val = 213!@#
# id = 2, val = 1.23923
# id = 3, val = c


```

**可迭代对象**

* 指可以使用 for 循环进行**遍历**的对象，有

  1. 列表（list）、元组（tuple）和字符串（str） 
  2.  集合（set）和字典（dict）中的键（keys）、值（values）或项（items） 
  3.  文件（file）对象 
  4. 生成器（generator）和 生成器表达式（generator expression）

* 解包操作( * )

  在可迭代对象前加上 * 操作符，解包后，可迭代对象中的每个元素都会**被拆分成单独的值**，并**按顺序**作为单独的位置参数传递给函数、方法或其他语句。如

  ```python
  def my_func(a, b, c):
      print(a, b, c)
  my_list = [1, 2, 3]
  my_func(*my_list)  
  # 输出：1 2 3
  # 而不是输出 [1, 2, 3]
  ```

  需要注意的是，只有**支持可变参数**的函数才能接受**解包后的可迭代对象**作为参数。



## 偏函数

定义：偏函数 (Partial function) 指由 `functools`模块 提供的 `partial` 方法，该方法把一个函数的某些参数给固定住（也就是**设置默认值**），**返回一个新的函数 newfunc**，调用 `newfunc`时将忽略已经被固定的参数（不接受传递）。

用法：`newfunc = functools.partial(函数名func, 参数arg1 = 固定值x1, ... , 参数argN = 固定值xN)`，随后调用 `newfunc` 时 `arg` 不再存在于参数列表中，`newfunc(arg1, arg2, ... )` 相当于 `func(arg1, arg2, ... , x)`。

优缺点：在多次调用多参数函数，且多次调用都对某些参数传递相同的参数值时，可以使用偏函数简化调用过程。缺点就是只能**从后面的参数开始连续固定**，而不能单单固定前面或中间的参数（这遵循*参数列表的有序性*）。

```python
import functools

def func(a, b, c, d):
    print(a + b + c + d)
    pass

func(1, 2, 3, 4)
newfunc1 = functools.partial(func, d = 100)
newfunc2 = functools.partial(func, c = 100, d = 10000)
# newfunc1() 固定传递给 func 的 d 参数值为 100
newfunc1(1, 2, 3)
# newfunc2() 固定传递给 func 的 c 参数值为 100，d 参数值为 10000
newfunc2(100, 200, 300)

for i in range(2, 36):
    my_int = functools.partial(int, base = i)
    # 固定以 i 进制表示数 10000
    print(my_int('10000'))
    pass
```



## 高阶函数

定义：高阶函数的参数列表中含有**函数参数**。自定义高阶函数时只需给定一个接收函数的参数即可。

python中内置四大高阶函数：

* `map()`: 接受一个函数和一个可迭代对象作为参数，将函数**应用于每个元素**，并返回一个所含元素与各个原元素一一对应新的可迭代对象。

```python
# 例如，将列表中的每个元素平方并返回新的列表可以使用如下代码：
lst = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, lst))
print(squared)  # Output: [1, 4, 9, 16, 25]
```

* `filter()`: 接受一个函数和一个可迭代对象作为参数，将函数**应用于每个元素**，并返回一个**仅包含满足条件的元素**的新的可迭代对象。

```python
# 例如，从列表中筛选出所有偶数可以使用如下代码：
lst = [1, 2, 3, 4, 5]
even_nums = list(filter(lambda x: x % 2 == 0, lst))
print(even_nums)  # Output: [2, 4]
```

* `reduce()`: 在 Python3 中，reduce 函数已经被移动到 functools 模块中。它接受一个函数和一个可迭代对象作为参数，对可迭代对象中的元素进行**累积操作**，并返回最终结果。

```python
# 例如，计算列表中所有元素的乘积可以使用如下代码：
from functools import reduce
lst = [1, 2, 3, 4, 5]
product = reduce(lambda x, y : x * y, lst)
print(product)  # Output: 120
```

* `sorted()`: 接受一个可迭代对象作为参数，并返回一个新的已排序的列表。如果需要**按照某个键或函数的返回值**进行排序，可以使用 `key` 参数传递一个函数。

```python
# 使用 sorted() 按不同函数参数 key 来对可迭代对象排序
lis = [{"name" : "abc", "age" : 18}, {"name" : "cde", "age" : 68}, {"name" : "def", "age" : 39}, {"name" : "fgh", "age" : 11}]

def getKey1(x):
    return x["name"]

def getKey2(x):
    return x["age"]

print(sorted(lis, key = getKey1))
# Output: [{'name': 'abc', 'age': 18}, {'name': 'cde', 'age': 68}, {'name': 'def', 'age': 39}, {'name': 'fgh', 'age': 11}]

print(sorted(lis, key = getKey2))
# Output: [{'name': 'fgh', 'age': 11}, {'name': 'abc', 'age': 18}, {'name': 'def', 'age': 39}, {'name': 'cde', 'age': 68}]
```

自定义高阶函数

```python
def cal(a, b, FunctionKey):
    print(FunctionKey(a, b))
    pass

def plus(a, b):
    return a + b

def minus(a, b):
    return a - b

def multiply(a, b):
    return a * b

def mypow(a, b):
    return a ** b

lis = [plus, minus, multiply, mypow]
a, b = 2, 10

for func in lis:
    cal(a, b, func)
    pass

# Output: 
# 12
# -8
# 20
# 1024
```



## 返回函数

定义：函数的返回值是一个函数，那么称该函数为返回函数。

用法：常结合函数闭包，即函数内部定义若干个函数，然后根据需求返回需要的函数。

```python
def cal(op):
    # 函数内部定义若干个函数（闭包），根据需求返回需要的函数
    # op == 0 -> sum 操作
    # op == 1 -> minus 操作
    # op == 2 -> multiply 操作
    
    def sum(*arg):
        res = 0
        for i in arg:
            res += i
        return res

    def minus(*arg):
        res = arg[0]
        for i in arg[1 : ]:
            res -= i
        return res

    def multiply(*arg):
        res = 1
        for i in arg:
            res *= i
        return res
    
    lis = [sum, minus, multiply]
    
    return lis[op]
    

a, b, c = 2, 5, 10

for i in range(0, 3):
    func = cal(i)
    print(func(a, b, c))
    pass

# Output:
# 17
# -13
# 100
```





## 匿名函数

> 匿名函数(lambda) 相当于一个**返回值为后面表达式 f(x) 结果**的函数，即自带 return f(x) 语句。匿名函数返回一个函数对象，可用一个变量接收并用该变量名调用。

用法:  `f = lambda 参数列表:表达式`，然后通过 `f(参数列表)` 来调用

优点：用于简化函数，便于封装

```python
f = lambda x, y : x + y
print(f(1, 3))

# 结合简化的条件语句，可用于简化二元运算
# 简化的条件语句解释：
a = 1
print("你是？" if a == 10 else "我是？")
# 上句相当于
# if a == 10:
#     print("你是？")
# else:
#     print("我是？")

# 结合简化条件语句：
f = lambda x, y : x if x > y else y
print(f(1, 3))

# 传递 lambda表达式 作为高阶函数的参数
lis = [{"name" : "abc", "age" : 18}, {"name" : "cde", "age" : 68}, {"name" : "def", "age" : 39}, {"name" : "fgh", "age" : 11}]

print(sorted(lis, key = lambda x : x["name"])) # 按键"name"的值排序
# Output: [{'name': 'abc', 'age': 18}, {'name': 'cde', 'age': 68}, {'name': 'def', 'age': 39}, {'name': 'fgh', 'age': 11}]

print(sorted(lis, key = lambda x : x["age"])) # 按键"age"的值排序
# Output: [{'name': 'fgh', 'age': 11}, {'name': 'abc', 'age': 18}, {'name': 'def', 'age': 39}, {'name': 'cde', 'age': 68}]
```



## 闭包

定义：如果在一个**内部函数**里对外部函数（不是在全局作用域）的**自由变量**（包括外部函数接受的参数）进行引用，内部函数就被认为是闭包。

理解：外部函数接收的参数及其作用下定义的自由变量将作为内部函数的**环境**，而内部函数本身就是个**函数块**，那么认为 **函数块 + 环境 = 闭包**。由闭包产生的函数对象仅在**环境**上有所区别，并且由同一闭包产生的不同函数对象都有自己**独立的作用域**。同时，同一函数对象的调用都将**共同影响该作用域**，并且每次调用都将受到*可能被影响过的作用域*影响，故调用同一函数对象时产生的函数结果将受其*之前的调用*影响。

形成闭包的条件：

- 必须包含一个嵌套函数（内部函数）
- 嵌套函数必须引用封闭函数（外部函数）中定义的自由变量
- 封闭函数必须返回嵌套函数

注意事项：

1. 被返回的函数不会立刻执行，而是直到被独立调用时才执行。
2. 闭包环境由外部函数接收的参数和其他自由变量所决定，每次通过闭包获得的函数对象，即使传递的参数变量相同，它们的闭包环境仍不相同，因为所处作用域不同，它们各自的作用域之间互不影响（除了全局变量等）。
3. 时刻注意各变量的作用域，以免在不经意的情况下创建新的临时变量。

常见错误：

1. 闭包的函数块中引用了循环变量（或者说函数块在某个循环结构中）或者后续会变化的变量。

   ```python
   # 修正前
   def count():
       funcs = []
       for i in range(1, 4):
           def f():
               return i * i
           funcs.append(f)
       return funcs
   
   # 修正后
   def __count():
       funcs = []
       for i in range(1, 4):
           def f(x = i): 
               return x * x
           funcs.append(f)
       return funcs
   
   
   func_lis = count()
   f1, f2, f3 = func_lis[0], func_lis[1], func_lis[2]
   
   
   __func_lis = __count()
   __f1, __f2, __f3 = __func_lis
   
   
   print(f1())
   print(f2())
   print(f3())
   # Output:
   # 9
   # 9
   # 9
   
   # 因为在把 3 个函数加入到 funcs 函数列表中前，发生了
   # 现象1：Python中，当循环结束以后，循环体中的临时变量 i 不会销毁，而是继续存在于执行环境中
   # 现象2：Python中的函数只有在被调用时才会将函数体内的变量值确定下来
   # 所以，对于 f1、f2、f3 来说，它们调用的 i 都是循环结束后 i 的值 3，即当前作用域的环境所决定的。
   
   
   
   print(__f1())
   print(__f2())
   print(__f3())
   # Output:
   # 1
   # 4
   # 9
   
   # 通过修改内部函数定义，保留住当时循环的每个 i 值
   # 此时对于 __f1 来说它的环境为 x = 1
   # def f(x = 1): 
   #     return x * x
   # 同理对于 __f2 来说它的环境为 x = 2
   # def f(x = 2): 
   #     return x * x
   ```

2. 在闭包函数块中修改了外部函数作用域中的变量

   ```python
   def func():
       a = 1
       def count():
           a = a + 1
           return a
       return count
   
   f = func()
   print(f())
   # 报错，无法输出
   # 原因：内部函数实质上定义了一个名为 a 的临时变量，当查找等号右式的 a 时发现未曾定义过（没有初值）导致报错，即内部函数的整个运作过程中都与外部函数的 a = 1 无关，任意形式的变量操作都将视为对内部函数的临时变量进行操作。
   
   
   # 修改方法1：使用关键字 nonlocal
   # nonlocal 作用：将最近层的外部函数中的变量 x 标记为内部函数的非局部变量，实质上就是使得该变量引用与 x 相同的内存地址，则内部函数内对 x 的修改将被应用到外部函数的作用域。
   def func():
       a = 1
       def count():
           nonlocal a
           a = a + 1
           return a
       return count
   
   f = func()
   print(f())
   # Output: 2
   
   
   
   # 修改方法2：使用容器
   # 原理：容器的调用不会产生歧义，除非该内存空间真的不存在才会报错
   def func():
       a = [1] # 以列表为例
       def count():
           a[0] = a[0] + 1
           return a[0]
       return count
   
   f = func()
   print(f())
   # Output: 2
   ```


总结：

1. 闭包中，如果要修改引用的外层变量 `x`：

   * 闭包函数块内使用 `nonlocal` 关键字更新该变量的作用域即 `nonlocal x`
   * 将外层变量当作内层函数的默认参数值即 `def inner(y = x)` ，则在内层函数中使用 `y` 代替显式使用 `x`

2. 注意不要在闭包中引用一个在外层函数后续会发生变化的变量，否则往往会得到非原先所期望的值：

   ```python
   # 例如
   def func():
       a = 1
       def inner():
           print(a)
           pass
       a = 2
       return inner
   
   f = func()
   f()
   # Output：2
   ```

   

应用：

```python
# 1.返回函数的简单闭包，这里闭包的自由变量即 exp
def myPower(exp = 1):
    def cal(x):
        return x ** exp
    return cal

pow2, pow3, pow4 = myPower(2), myPower(3), myPower(4)

num = 4
print("{a}^2 = {}, {a}^3 = {}, {a}^4 = {}".format(pow2(num), pow3(num), pow4(num), a = num))
# Output: 4^2 = 16, 4^3 = 64, 4^4 = 256



# 2.利用闭包特性：当闭包执行完后，仍然能够保持住当前的运行环境（即闭包作用域的持续性）。

# 坐标原点
ori = [0, 0]
# xOy平面坐标系上的四种方向
dir_x_positive = [1, 0]
dir_y_positive = [0, 1]
dir_x_negative = [-1, 0]
dir_y_negative = [0, -1]

def Point_create(setpos):
    pos = setpos.copy() # 复制一个内容与 setpos 列表相同的新列表（存在于新开辟的内存空间中）
    # 若直接 pos = setpos，则会导致创建的所有点共用同一个坐标点 pos

    # direction 表示运动方向（即某方向上的单位坐标）
    # step 表示在该方向上的运动距离
    def Point_move(direction, step):
        nx = pos[0] + direction[0] * step
        ny = pos[1] + direction[1] * step
        # 不能写成 pos = [nx, ny]，会被当成创建新的临时变量
        pos[0] = nx
        pos[1] = ny
        return pos
    
    return Point_move

pt1 = Point_create(ori)
print(pt1(dir_x_positive, 10))  # 点1 在 x 轴正方向走 10 步   -- [10, 0]
print(pt1(dir_y_negative, 20))  # 点1 在 y 轴负方向走 20 步   -- [10, -20]

pt2 = Point_create(ori)
print(pt2(dir_x_negative, 100)) # 点2 在 x 轴负方向走 100 步  -- [-100, 0]
print(pt2(dir_y_positive, 50))  # 点2 在 y 轴正方向走 50 步   -- [-100, 50]
```



## 装饰器







---

# 三、类和对象

---

> 类是模板，对象是根据类这个模板创建出来的。类只有一个，而对象可以有很多个。类名通常采用**大驼峰命名法**（每个有意义的单词首字母大写），如 `CapWords`。

类和对象的创建：

```python
class ClassName:
    # 类属性 及其 初始值
    ···
    
    # 内置方法
    def __new__(cls):
        return object.__new__(cls);
    def __init__(self):
        pass
    ···
    
    # 实例方法
    def f1(self):
        pass
    def f2(self):
        pass
    ···
    
    
c1 = ClassName()
c1.f1()
c1.f2()
···
```

注意：类中实例方法的参数列表必须至少包含参数 `self`，且必须为**首个参数**。`self` 表示**实例对象本身**，类似于 C/C++ 的 `this` 关键字。

内置方法：

凡是在类内部定义，以 `__NAME__`的形式命名的方法，都是类的内置方法，也称之为魔法方法。常用的有：

* `__init__(self[, args...])`：**构造方法**，在创建对象时自动调用，重写构造函数用于自定义**初始化实例变量**。

* `__new__(cls[, args...])` 在`__init__`触发**前**自动调用，用于产生实例化对象，且必须返回一个**对象**`object.__new__(cls[, args...])`。若重定义时不返回一个对象，此时的对象为空对象即 `None`，且导致实例方法报错（传入了空对象）。

  *注意：如果重定义的`__init__`中有除了 `self` 外的参数，则 `__new__` 的参数列表应为 `__new__(cls, *args, **kwargs)` 以接收这些参数。*

* `__del__(self[, args...])`：**析构方法**，当对象在内存中被释放时，自动触发执行。析构函数的调用一般是由解释器在进行垃圾回收时自动触发执行的，无需自行定义。

* `__str__(self[, args...])`：返回对象的**自定义字符串表示形式**。当使用`print()`函数输出实例对象时，会自动调用该方法，若不进行自定义而直接打印对象则默认会输出对象的内存地址。该方法限制返回值为**一个字符串**。

* `__class__`：返回一个对象所属的类，可以被用于任何对象上，包括实例对象、类对象和类型对象。

  假设有一个类 `MyClass` 和一个该类的实例 `obj`，我们可以使用 `obj.__class__` 来获取这个实例所属的类即输出` <class '__main__.MyClass'>`，也可以使用 `MyClass.__class__` 来获取这个类所属的类（即类型对象 `type`）即输出 `<class 'type'>`。在实际编程中，`__class__` 可以用来**判断对象是否属于某个类或其子类**，或者获取对象的**类型信息**等。不过需要注意的是，直接修改 `__class__` 的值可能会导致程序出错或产生意料之外的后果，因此一般情况下并不建议这么做。

```python
# 旧版 python 创建类的写法 class Cat(object):
# python 3 之后就不需要显式地写出参数 object
class Cat:
    # 类属性
    # 必须附带空属性值
    name = "c1"
    age = 0
    sex = '雄'
    exist = False

    # 内置方法
    def __new__(cls, *args, **kwargs):
        return object.__new__(cls)
    
    def __init__(self, name = "c1", age = 0, sex = '雄'):
        self.exist = True
        self.name = name
        self.age = age
        self.sex = sex
        print("一个 Cat 对象被构造")
        pass

    def __del__(self):
        print("一个 Cat 对象被销毁")
        pass

    def __str__(self):
        return "一只{}猫 {}，它已经 {} 岁了".format(self.sex, self.name, self.age)

    # 实例方法
    # 不同于其他语言，在类内部的方法必须使用 self 引用成员属性
    def f1(self):
        print(self.name) # 不能直接 print(name)
        pass

    def f2(self):
        print(self.age)
        pass
    
    def Intro(self):
        print("一只{}猫 {}，它已经 {} 岁了".format(self.sex, self.name, self.age))
        pass


c1 = Cat()
c1.f1()
c1.f2()


c2 = Cat("Tom", 5, '雌')
# 此时以下两句输出结果相同
print(c2)
c2.Intro()
```

## 继承

> 继承是一种创建新类的方式，新建的类可称为**子类**或派生类，**父类**可称为基类或超类。python支持**多继承**，即新建的类可以支持**一个或多个父类**。`object`默认是所有类的父类（超类）。继承具有**传递性**，子类可继承到其父类的所有父类（一般最多传递三级，否则代码可读性大打折扣）。

作用：父类拥有的成员和方法子类都会继承并拥有，而子类拥有的成员和方法父类则不一定有。多个子类的相同属性和行为都将从同一个父类中继承，大大提高编码效率，并且便于统一管理和修改。

注意：若子类拥有**与父类同名的方法**，则子类的该方法会**覆盖**掉父类的，因为最先搜索到的自然是子类本身的方法，即调用该方法名时仅会调用到子类的方法内容。

### 经典类与新式类

在对于同名方法的搜寻顺序上的区别：

```python
class G:
    def test(self):
        print('from G')
class E(G):
    def test(self):
        print('from E')
class F(G):
    def test(self):
        print('from F')
class B(E):
    def test(self):
        print('from B')
class C(F):
    def test(self):
        print('from C')
class D(G):
    def test(self):
        print('from D')

class A(B, C, D):
    pass


obj = A()
obj.test()
# 经典类 
# 查找顺序为 : obj -> A -> B -> E -> G -> C -> F -> D -> object
# 输出 from B

# 新式类
# 查找顺序为 : obj -> A -> B -> E -> C -> F -> D -> G -> object
# 输出 from D
```

* 经典类：按深度优先顺序查找（在 python 2 中，没有继承 `object` 的类及其子类都是经典类）

  <img src="D:\MarkdownText\image_python\9.png" alt="9" style="zoom: 67%;" />

* 新式类：按广度优先顺序查找（在 python 3 中，均默认为新式类）

  <img src="D:\MarkdownText\image_python\10.png" alt="10" style="zoom: 67%;" />

### super() 方法

super() 方法的存在就是为了解决多重继承的问题，可以在**一个父类中**使用 super() 方法用于调用**下一个父类**的方法，也可以在**子类**中使用 super() 方法用于按新式类顺序寻找**首个同名的父类方法**并执行（若参数列表不匹配则会报错）。好处就是可以避免直接使用父类的名字。

使用：

* python 2：`super(子类，self).父类方法名`
* python 3：`super().父类方法名` 省略了括号中的参数，且后续父类方法的参数列表将**省略 `self` 参数**



```python
class A:
    def test(self):
        print('from A')
        super().test() # 省略参数 self
'''用于调用C的下一个父类的方法B.test()'''

class B:
    def test(self):
        print('from B')


class C(A, B):
    pass


c = C()
c.test()
print(C.mro())
# 查找顺序如下
#[<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>, <class 'object'>]
#即 C 的第一个父类为 A，则下一个父类为 B

# 结果
# from A
# from B
```



### **单继承与多继承**

```python
class Parent1:
    pass
class Parent2:
    pass

class Sub1(Parent1): #单继承
    pass
print(Sub1.__bases__)  # 查看自己的父类---->(<class '__main__.Parent1'>,)

class Sub2(Parent1,Parent2): # 多继承
    pass
print(Sub2.__bases__)    # 查看自己的父类---->(<class '__main__.Parent1'>, <class '__main__.Parent2'>)



#单继承
class Animal:

    def walk(self):
        print("A {} is walking now".format(type(self)))
    pass

class Cat(Animal):
    def meow(self):
        print("Cat Meow")
    pass

class Dog(Animal):
    def bark(self):
        print("Dog Bark")
    pass

c = Cat()
d = Dog()
c.meow()
d.bark()
c.walk()
d.walk()
'''
Cat Meow
Dog Bark
A <class '__main__.Cat'> is walking now
A <class '__main__.Dog'> is walking now
'''


#多继承
class Creature:
    name = ""
    exist = True
    def __init__(self, name):
        self.name = name;
        pass
    def isExist(self):
        print("{} is existed".format(self.name))
    def setName(self, nm):
        self.name = nm
    pass

class Animal:
    # 若这里有同名方法
    # def __init__(self):
    # 	 pass
    # 但是下面 super()引用的 __init__ 方法具有参数 name，则程序会报错（不会重新往后面再寻找同名方法了）
    def walk(self):
        print("A {} is walking now".format(type(self)))
    pass

class Cat(Animal, Creature):
    def __init__(self, name):
        # Creature.__init__(self, name) # 与下句等效
        super().__init__(name) # 从父类中自动寻找可执行方法 __init__(self, name) 的父类，这里就找到了父类 Creature
        # 这里的寻找机制是首个对应参数列表
        pass
    def meow(self):
        print("Cat Meow")
    pass

class Dog(Animal, Creature):
    def bark(self):
        print("Dog Bark")
    pass


c = Cat("Cat1")
d = Dog("Dog1")
c.isExist()
d.isExist()
# Cat1 is existed
# Dog1 is existed



#类方法调用的先后顺序
#从子类开始找一层一层往父类找，同层次类找完才会开始往上一层父类找。
class A:
    def isCalled(self):
        print("A is called")
    pass
class B(A):
    pass
class C(A):
    def isCalled(self):
        print("C is called")
    pass
class D(B, C):
    pass

d = D()
d.isCalled()
# 输出 C is called
# 故系统查找 isCalled 方法的顺序为 D -> B -> C -> A

print(D.__mro__) # 输出类的依次继承关系（从子类到父类最终到默认超类 object），也展示了同名方法的搜索顺序，仅执行最先找到的。
# (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)
```



## 类属性与实例属性

* 类属性：是在**类中**定义的属性，它是和这个类所绑定的，这个类中的**所有对象**都可以访问。访问时可以通过**类名**来访问，也可以通过**实例名**来访问。
* 实例属性：是与**类的实例**相关联的数据值，是这个实例**私有**的，**只有这个对象自己可以访问**。当一个实例被释放后，它的属性同时也被清除了。
* 所以，实例属性只能通过实例对象访问，而类属性可通过类名或者实例对象访问。

```python
class Student:
    # 类属性
    name = "abc"

    def __init__(self, age):
        self.age = age #实例属性
        pass
    pass

stu = Student(18)

print(stu.name)
print(stu.age)

print(Student.name)
# print(Student.age) 报错
```

---

# 四、常见包

---







## pathlib 

> 该模块提供了表示**文件系统路径的类**，可适用于不同的操作系统。使用 pathlib 模块，相比于 os 模块可以写出更简洁，易读的代码。pathlib 模块中的 Path 类继承自 PurePath，对 PurePath 中的部分方法进行了重载，相比于 os.path 有更高的抽象级别。

**Path 和 PurePath** 

* Path：Path访问实际文件系统的**“真正路径”**，Path对象可用于判断对应的文件是否存在、是否为文件、是否为目录等。有两个子类，即PosixPath和WindowsPath，前者用于操作UNIX（包括 Mac OS X）风格的路径，后者用于操作Windows风格的路径。

* PurePath：PurePath访问实际文件系统的**“纯路径”**，只负责对**路径字符串**执行操作。PurePath有两个子类，即PurePosixPath和PathWindowsPath，前者用于操作UNIX（包括 Mac OS X）风格的路径，后者用于操作Windows风格的路径。

* 区别：

  Path 是 PurePath 的子类，除了支持 PurePath 的各种操作、属性和方法之外，还会真正访问底层的文件系统，包括判断 Path 对应的路径是否存在，获取 Path 对应路径的各种属性（如是否只读、是文件还是文件夹等），甚至可以对文件进行读写。

  PurePath 和 Path 最根本的区别在于，PurePath 处理的仅是字符串，而 Path 则会真正访问底层的文件路径，因此它提供了属性和方法来访问底层的文件系统。

### 具体操作

| pathlib操作            | os及os.path操作      | 功能描述                                                     |
| ---------------------- | -------------------- | ------------------------------------------------------------ |
| Path.resolve()         | os.path.abspath()    | 获得绝对路径                                                 |
| Path.chmod()           | os.chmod()           | 修改文件权限和时间戳                                         |
| Path.mkdir()           | os.mkdir()           | 创建目录                                                     |
| Path.rename()          | os.rename()          | 文件或文件夹重命名，如果路径不同，会移动并重新命名           |
| Path.replace()         | os.replace()         | 文件或文件夹重命名，如果路径不同，会移动并重新命名，如果存在，则破坏现有目标。 |
| Path.rmdir()           | os.rmdir()           | 删除目录                                                     |
| Path.unlink()          | os.remove()          | 删除一个文件                                                 |
| Path.unlink()          | os.unlink()          | 删除一个文件                                                 |
| Path.cwd()             | os.getcwd()          | 获得当前工作目录                                             |
| Path.exists()          | os.path.exists()     | 判断是否存在文件或目录name                                   |
| Path.home()            | os.path.expanduser() | 返回电脑的用户目录                                           |
| Path.is_dir()          | os.path.isdir()      | 检验给出的路径是一个目录                                     |
| Path.is_file()         | os.path.isfile()     | 检验给出的路径是一个文件                                     |
| Path.is_symlink()      | os.path.islink()     | 检验给出的路径是一个符号链接                                 |
| Path.stat()            | os.stat()            | 获得文件属性                                                 |
| PurePath.is_absolute() | os.path.isabs()      | 判断是否为绝对路径                                           |
| PurePath.joinpath()    | os.path.join()       | 连接目录与文件名或目录                                       |
| PurePath.name          | os.path.basename()   | 返回文件名                                                   |
| PurePath.parent        | os.path.dirname()    | 返回文件路径                                                 |
| Path.samefile()        | os.path.samefile()   | 判断两个路径是否相同                                         |
| PurePath.suffix        | os.path.splitext()   | 分离文件名和扩展名                                           |



### 使用方法

首先导入模块

```python
from pathlib import *
```

#### 获取路径

1. 传入字符串

   在创建PurePath和Path时，既可以传入单个字符串，也可传入多个路径字符串，PurePath会将它们拼接成一个字符串。

   ```python
   # 以下结果均指返回值
   
   PurePath('M工具箱','MTool工具/示例','info')
   # 结果： PureWindowsPath('M工具箱/MTool工具/示例/info')
   
   Path('M工具箱')
   # 结果： WindowsPath('M工具箱')
   
   input_path = r"C:\Users\okmfj\Desktop\MTool工具"
   Path(input_path)
   # 结果： WindowsPath('C:/Users/okmfj/Desktop/MTool工具')
   ```

2. 获取目录

   - `Path.cwd()`，返回文件当前所在目录；
   - `Path.home()`，返回电脑用户的目录。

   ```python
   Path.cwd()
   # 结果： WindowsPath('D:/M工具箱')
   
   Path.home()
   # 结果： WindowsPath('C:/Users/okmfj')
   ```

3. 目录拼接

   拼接出Windows桌面路径，当前路径下的子目录或文件路径。

   ```python
   # 拼接出Windows桌面路径
   # 法1
   Path(Path.home(), "Desktop")
   # 结果： WindowsPath('C:/Users/okmfj/Desktop')
   
   # 法2
   Path.joinpath(Path.home(), "Desktop")
   # 结果： WindowsPath('C:/Users/okmfj/Desktop')
   
   
   
   # 用重载后的 '/' 运算符拼接出当前路径下的“MTool工具”子文件夹路径
   res = Path.cwd() / 'MTool工具'
   # 结果： WindowsPath('D:/M工具箱/MTool工具')
   ```



#### 路径处理

获取路径的不同部分、或不同字段等内容，用于后续的路径处理，如拼接路径、修改文件名、更改文件后缀等。

```python
from pathlib import *

input_path = r"C:\Users\okmfj\Desktop\MTool工具"

Path(input_path).name  # 返回文件名+文件后缀
Path(input_path).stem  # 返回文件名
Path(input_path).suffix  # 返回文件后缀
Path(input_path).suffixes  # 返回文件后缀列表
Path(input_path).root  # 返回根目录
Path(input_path).parts  # 返回文件
Path(input_path).anchor  # 返回根目录
Path(input_path).parent  # 返回父级目录
Path(input_path).parents  # 返回所有上级目录的列表
```



#### 路径判断

- `Path.exists()`，判断 Path 路径是否是一个已存在的文件或文件夹；
- `Path.is_dir()`，判断 Path 是否是一个文件夹；
- `Path.is_file()`，判断 Path 是否是一个文件。

```python
from pathlib import *

input_path = r"C:\Users\okmfj\Desktop\MTool工具"

if Path(input_path ).exists():
	if Path(input_path ).is_file():
		print("是文件！")
	elif Path(input_path ).is_dar():
		print("是文件夹！")
else:
	print("路径有误，请检查!")
```



#### 路径建立、删除

- `Path.mkdir()`，创建文件夹；
- `Path.rmdir()`，删除文件夹，文件夹必须为空；
- `Path.unlink()`，删除文件。

```python
from pathlib import *

input_path = r"C:\Users\okmfj\Desktop\MTool工具"

# 创建文件夹函数
def creat_dir(dir_path):
	if Path(dir_path).exists():  # 如果已经存在，则跳过并提示
		print("文件夹已经存在！")
	else:
		Path.mkdir(Path(dir_path))  # 创建文件夹

creat_dir(input_path )  # 调用函数

# 删除文件夹函数
def del_dir(dir_path):
	if Path(dir_path).exists():  # 如果存在，则删除文件夹
		Path.rmdir(Path(dir_path))
	else:
		print("文件夹不存在！")  # 文件夹不存在

del_dir(input_path )  # 调用函数

# 删除文件函数
def del_file(dir_path):
	if Path(dir_path).exists():  # 如果存在，则删除文件
		Path.unlink()(Path(dir_path))
	else:
		print("文件不存在！")  # 文件不存在

del_file(input_path )  # 调用函数
```



#### 路径匹配查找（迭代）

- `Path.iterdir()`，查找文件夹下的所有文件，返回的是一个生成器类型；

  ```python
  from pathlib import *
  
  input_path = r"C:\Users\okmfj\Desktop\MTool工具"
  
  # 获取文件夹下所有文件，返回文件路径（字符）列表，采用iterdir方法
  [str(f) for f in Path(x).iterdir() if Path(f).is_file()]
  
  # 获取文件夹下所有文件类型，返回文件后缀的列表，采用iterdir方法
  list(set([Path(f).suffix for f in Path(x).iterdir() if Path(f).is_file()]))
  # 结果： ['.pdf', '.txt', '.docx']
  ```

- `Path.glob(pattern)`，查找文件夹下所有与 pattern 匹配的文件，返回的是一个生成器类型；

  ```python
  from pathlib import *
  
  input_path = r"C:\Users\okmfj\Desktop\MTool工具"
  
  # 获取文件夹下所有文件，返回文件路径（字符）列表，采用glog方法
  [str(f) for f in Path(input_path).glob(f"*.*") if Path(f).is_file()]
  
  # 获取文件夹下所有文件，返回文件路径（字符）列表，采用glog方法
  [str(f) for f in Path(input_path).glob(f"**\*.*") if Path(f).is_file()]
  
  # 获取文件夹下所有文件类型，返回文件后缀的列表，采用glog方法
  list(set([Path(f).suffix for f in Path(input_path).glob(f"*.*") if Path(f).is_file()]))
  # 结果： ['.pdf', '.txt', '.docx']
  
  # 获取文件夹下（含子文件）所有文件类型，返回文件后缀的列表，采用glog方法
  list(set([Path(f).suffix for f in Path(x).glob(f"**\*.*") if Path(f).is_file()]))
  # 结果： ['.pdf', '.txt', '.docx']
  ```

  

- `Path.rglob(pattern)`，查找文件夹下所有子文件夹中与 pattern 匹配的文件，返回的是一个生成器类型。

  ```python
  from pathlib import *
  
  input_path = r"C:\Users\okmfj\Desktop\MTool工具"
  
  # 获取文件夹下（含子文件）所有文件，返回文件路径(字符)列表，采用rglog方法
  [str(f) for f in Path(x).rglob(f"*.*") if Path(f).is_file()]
  
  # 获取文件夹下（含子文件）所有文件类型，返回文件后缀的列表，采用rglog方法
  list(set([Path(f).suffix for f in Path(x).rglob(f"*.*") if Path(f).is_file()]))
  # 结果： ['.pdf', '.txt', '.docx']
  ```

  

#### 读、写文件

- `Path.open(mode='r')`，以 "r" 格式打开 Path 路径下的文件，若文件不存在即创建后打开。
- `Path.read_bytes()`，打开 Path 路径下的文件，以字节流格式读取文件内容，等同 open 操作文件的 "rb" 格式。
- `Path.read_text()`，打开 Path 路径下的文件，以 str 格式读取文件内容，等同 open 操作文件的 "r" 格式。
- `Path.write_bytes()`，对 Path 路径下的文件进行写操作，等同 open 操作文件的 "wb" 格式。
- `Path.write_text()`，对 Path 路径下的文件进行写操作，等同 open 操作文件的 "w" 格式。

![读、写模式汇总表](https://pic4.zhimg.com/80/v2-6575c60a57fbf05cf64144b74d8f03ab_1440w.webp)

```python
from pathlib import *

input_path = r"C:\Users\okmfj\Desktop\MTool工具\1.txt"

with Path(input_path).open('w') as f:  # 创建并打开文件
	f.write('M工具箱')  # 写入内容
f = open(input_path, 'r')  # 读取内容
print(f"读取的文件内容为：{f.read()}")
f.close()
```



#### 属性和方法汇总

![img](https://pic3.zhimg.com/80/v2-dfcb4f5442973aa7f8058075f949660e_1440w.webp)

![img](https://pic2.zhimg.com/80/v2-2983703aa2b4632016e25c4fe5a8b0d9_1440w.webp)



## pandas

[pandas数据分析总结大全（入门加进阶） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/142556037)



