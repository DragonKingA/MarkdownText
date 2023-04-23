# Python

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

``` python
l1 = [1, 2, 3]
l2 = [1, 2, 0]

# 1. all() 判断元组或列表（可迭代参数 iterable）中所有元素是否都为 True，是则返回 True，否则返回 False
# 除了 0、空、False 外都算 True，也可以理解为数据强转为 bool 类型后的值。
all(l1) == True
all(l2) == False

# 2. any() 判断元组或列表（可迭代参数 iterable）中所有元素是否至少一个为 True，是则返回 True，否则返回 False
any(l1) == any(l2) == True
any([0, 0, 0, "", False]) == False

# 3. sorted()

```

