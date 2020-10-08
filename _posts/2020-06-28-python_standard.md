---
layout: post
title: python标准库学习笔记
date: 2020-06-28
Author: karinlee
categories: 
tags: [python]
comments: true
---


### 0 知识

#### 优先级
 - not or and 的优先级是不同的：not > and > or
 - 幂运算操作符比其左侧的一元操作符优先级高，比其右侧的一元操作符优先级低


#### 序列特点
- 根据列表、元组和字符串的共同特点，把它们三统称为序列，因为他们有以下共同点：


```
    1）都可以通过索引得到每一个元素
	2）默认索引值总是从0开始（当然灵活的Python还支持负数索引）
	3）可以通过分片的方法得到一个范围内的元素的集合
	4）有很多共同的操作符（重复操作符、拼接操作符、成员关系操作符）
```


​    
#### 打印函数帮助
**使用 `FunctionName.__doc__`打印帮助**

实例：

```
def function_one():
    """ 这里是函数注释"""
    pass
    
print(function_one.__doc__)
```

输出：  
     

```
     这里是函数注释
```



#### lambda匿名函数

实例：
```
 # lambda x, y: x*y；函数输入是x和y，输出是它们的积x*y
 

>>>lam = lambda x, y: x*y
>>>lam(3,4)
>>>12
```

#### id(obj, /)可以查看对象在内存里的地址

实例：
```
>>>l1 =[1,2,3,4,5]
>>>id(l1)
2650382966216
```


####  assert关键字

assert这个关键字我们称之为“断言”，当这个关键字后边的条件为假的时候，程序自动崩溃并抛出    AssertionError的异常。
什么情况下我们会需要这样的代码呢？当我们在测试程序的时候就很好用，因为与其让错误的条件导致程序今后莫名其妙地崩溃，不如在错误条件出现的那一瞬间我们实现“自爆”。
一般来说我们可以用Ta再程序中置入检查点，当需要确保程序中的某个条件一定为真才能让程序正常工作的话，assert关键字就非常有用了。
	
实例：


```
>>> assert 1 + 1 == 2
>>> assert isinstance('Hello', str)    #  assert后表达式为True时，没有情况发生
>>> assert isinstance('Hello', int)     #  因为'Hello'是一个str，所以 isinstance('Hello', int)返回False
# 当assert后表达式为False时 程序崩溃报错AssertionError
Traceback (most recent call last):
  File "<input>", line 1, in <module>
AssertionError
```

#### 成员资格运算符 in

 Python 有一个成员资格运算符：in，用于检查一个值是否在序列中，如果在序列中返回 True，否则    返回 False。

实例：


```
>>> name = '小金鱼'
>>> '鱼' in name
True
>>> '肥鱼' in name
False
```

###  1 python常用函数


####  取得一个变量的类型，可以使用 type() 和 isinstance()

实例：
```
>>>type(6)
>>> <class 'int'>
>>>type('python')
>>> <class 'str'>

>>>a = 2
>>> isinstance (a,int)
True
>>> isinstance (a,str)
False
>>> isinstance (a,(str,int,list))    # 是元组中的一个返回 True
True
```




#### enumerate() 函数

enumerate() 函数用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标，一般用在 for 循环当中。
该函数返回 enumerate(枚举) 对象。
 	

实例：
	

```
>>>seasons = ['Spring', 'Summer', 'Fall', 'Winter']
>>>enumerate(seasons)
<enumerate object at 0x000002550C53D558>
>>>list(enumerate(seasons))
[(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
>>>list(enumerate(seasons,start =1))
[(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]

```

#### 可迭代对象转换为列表、元组和字符串

`list([iterable])` 把可迭代对象转换为列表

`tuple([iterable])` 把可迭代对象转换为元组

`str(obj)`  把对象转换为字符串

实例：

```
>>> temp = 'I love FishC.com!'
>>> list(temp)
['I', ' ', 'l', 'o', 'v', 'e', ' ', 'F', 'i', 's', 'h', 'C', '.', 'c', 'o', 'm', '!']
```


#### filter() 函数


filter() 函数用于过滤序列，过滤掉不符合条件的元素，返回由符合条件元素组成的新列表。该接收两个参数，第一个为函数或None，第二个为序列，序列的每个元素作为参数传递给函数进行判断，然后返回 True 或 False，最后将返回 True 的元素放到**迭代器**（python3.x）中。

语法：`filter(function or None,iterable)`


实例1 ：

```
>>>f = filter(None,[1,0,False,None,3])
>>>l = list(f)
>>>print(f)
<filter object at 0x000002555AE5C828>
>>>print(l)
[1, 3]
```

实例2：

```
# 过滤出列表的所有奇数
def is_odd(n): 
    return n % 2 == 1 
newlist = filter(is_odd, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
print(newlist)

```
输出：
```
[1, 3, 5, 7, 9]
```


#### map() 函数


map() 会根据提供的函数对指定序列做映射。第一个参数 function 以参数序列中的每一个元素调用 function 函数，返回包含每次 function 函数返回值的**迭代器**（python3.x）。

语法：`map(function, *iterables)`

实例：


```
>>>def square(x):
...     return x ** 2
... 
>>>map(square, [1,2,3,4,5]) 
<map object at 0x000002555AE5CC18>
>>>list(map(square, [1,2,3,4,5])) 
[1, 4, 9, 16, 25]

>>>map(lambda x, y: x + y, [1, 3, 5, 7, 9], [2, 4, 6, 8, 10])
<map object at 0x000002555AE5CCF8>
>>>list(map(lambda x, y: x + y, [1, 3, 5, 7, 9], [2, 4, 6, 8, 10]))
[3, 7, 11, 15, 19]
```






### 2 字符串

#### 字符串格式化符号

|  符   号 |  说     明|
|--|--|
| %c | 格式化字符及其ASCII码 |
| %s |  格式化字符串 |
|%d| 格式化整数|
| %o | 格式化无符号八进制数 |
| %x |  格式化无符号十六进制数 |
| %f | 格式化定点数，可指定小数点后的精度 |


#### split()方法

split() 通过指定分隔符对字符串进行切片，如果第二个参数 num 有指定值，则分割为 num+1 个子字符串。
语法 ：`str.split(str="", num=string.count(str))`
实例：

```
>>>str1 = "a,b,c,d"
>>>str1.split(',')
['a', 'b', 'c', 'd']

```

#### 若s为字符串，有以下函数可以对字符串作出判断


`s.isalnum()` 所有字符都是数字或者字母，为真返回 True，否则返回 False。

`s.isalpha()`   所有字符都是字母，为真返回 True，否则返回 False。

`s.isdigit()`     所有字符都是数字，为真返回 True，否则返回 False。

`s.islower()`    所有字符都是小写，为真返回 True，否则返回 False。

`s.isupper()`   所有字符都是大写，为真返回 True，否则返回 False。

`s.istitle()`      所有单词都是首字母大写，为真返回 True，否则返回 False。

`s.isspace()`   所有字符都是空白字符，为真返回 True，否则返回 False。



严格解析：有除了数字或者字母外的符号（空格，分号,etc.）都会False


`isalnum()`必须是数字和字母的混合
`isalpha()`不区分大小写



 实例：

```
>>> s = 'HELLOPYTHON'
>>> s.isupper()
>>> True
```

### 3列表

#### 向列表增加元素
 append() 方法是将参数作为一个元素增加到列表的末尾。
extend() 方法则是将参数作为一个列表去扩展列表的末尾。
insert()方法可以在列表的指定位置插入元素，语法：`list.insert(index, obj)`


实例：

```
>>> name = ['F', 'i', 's', 'h']
>>> name.append('C')
>>> name
['F', 'i', 's', 'h', 'C']
>>> name.extend(['.', 'c'])
>>> name
['F', 'i', 's', 'h', 'C', '.', 'c']
>>> name.append(['o', 'm'])
>>> name
['F', 'i', 's', 'h', 'C', '.', 'c', ['o', 'm']]
>>>name.insert(2,'hi')
>>>name
['F', 'i', 'hi', 's', 'h', 'C', '.', 'c', ['o', 'm']]
```


### 4 字典


python中，键一般是唯一的，如果重复最后的一个键值对会替换前面的，值不需要唯一。

#### 创建字典的几种方式

```
# 1
# 传入2维元组或列表 （有一一对应关系）
>>>dict1 = dict((('f',70),('i',105)))
>>>dict1
{'f': 70, 'i': 105}
```

```
# 2  
# 注意：键 苹果 和 橙子 不需要引号，创建字典后自动会加引号成为字符串

>>>dict2 = dict( 苹果='apple',橙子='orange')
>>>dict2
{'苹果': 'apple', '橙子': 'orange'}
```

  #### 修改或添加字典项
  语法 ：`dict[key] = value`    修改（存在键）或新增（不存在键）的项


  实例：
```
>>>dict2 = {'苹果': 'apple', '橙子': 'orange'}
>>>dict2['苹果'] = 'APPLE' 
>>>dict2['西瓜']= 'watermelon'
>>>dict2
{'苹果': 'APPLE', '橙子': 'orange', '西瓜': 'watermelon'}

```


#### fromkeys() 函数
Python 字典 fromkeys() 函数用于创建一个新字典，以序列 seq 中元素做字典的键，value 为字典所有键对应的初始值。
语法 ：
`dict.fromkeys(seq[, value])`
实例：

```
>>>dict3 = {}
>>>dict3.fromkeys((1,2,3),"hello")
{1: 'hello', 2: 'hello', 3: 'hello'}
>>>dict4 = {}
>>>dict4.fromkeys((1,2,3),(4,5,6))
{1: (4, 5, 6), 2: (4, 5, 6), 3: (4, 5, 6)}
```

注意：fromkeys()函数没有修改字典，只是新建了一个字典

#### 访问字典

dict.keys() 返回键
dict.values()返回值
dict.items() 返回项  （元组形式）

实例：
```
>>>dict5 = {'苹果': 'APPLE', '橙子': 'orange', '西瓜': 'watermelon'}
>>>dict5.keys()
dict_keys(['苹果', '橙子', '西瓜'])
>>>type(dict5.keys())
<class 'dict_keys'>
>>>for key in dict5.keys():
...        print(key)
... 
苹果
橙子
西瓜
>>>dict5.values()
dict_values(['APPLE', 'orange', 'watermelon'])
>>>type(dict5.values())
<class 'dict_values'>
>>>for value in dict5.values():
...        print(value)
...     
APPLE
orange
watermelon

>>>dict5.items()
>>>dict_items([('苹果', 'APPLE'), ('橙子', 'orange'), ('西瓜', 'watermelon')])
>>>for item in dict5.items():
...        print(item)
...     
('苹果', 'APPLE')
('橙子', 'orange')
('西瓜', 'watermelon')

>>>for key,value in dict5.items():
...        print(key,value)
...     
苹果 APPLE
橙子 orange
西瓜 watermelon
>>>for item in dict5.items():
...        for e in item:
...            print(e)
...         
苹果
APPLE
橙子
orange
西瓜
watermelon

```

可以采用`dict[key]` 或者 `dict.get(key)`的方式通过键来查找值
如果key不存在，` dict[key]`会报错` dict.get(key)` 返回None

```
>>>dict5 = {'苹果': 'APPLE', '橙子': 'orange', '西瓜': 'watermelon'}
>>>dict5['苹果']
'APPLE'
>>>dict5.get('苹果')
'APPLE'
>>>dict5['菠萝']
Traceback (most recent call last):
     File "<input>", line 1, in <module>
KeyError: '菠萝'
>>>dict5.get('菠萝')
>>>print(dict5.get('菠萝'))
None 


```


### 5 I/O

打开文件

f = open("filename.txt",encoding="utf8")
或者 
f = open("filename.txt",encoding="utf-8")
也是一样的

`os.path.basename(filePath)` 将全文件名提取为普通文件名

将字符串序列写入txt文件中，可以采用`f.writelines(seq)`方法

实例：
```
f = open("test.txt","w")
l = ["苹果"，“橙子”,"香蕉"]
f.writelines(l)
f.close()

```