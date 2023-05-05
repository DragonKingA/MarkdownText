# Python

---

# 零、输入与输出

---

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

## 匿名函数 lambda

> 匿名函数(lambda) 相当于一个**返回值为后面表达式 f(x) 结果**的函数，即自带 return f(x) 语句

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

#结合简化条件语句：
f = lambda x, y : x if x > y else y
print(f(1, 3))
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

### 经典类与新式类区别

在对于同名方法的搜寻顺序上：

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



