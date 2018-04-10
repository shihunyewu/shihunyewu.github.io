---
layout:     post
title:      "Python-Note"
subtitle:   " \"python核心编程学习笔记\""
date:       2018-04-03 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Python
---

> 我以前只系统地看过Python的菜鸟教程，而菜鸟教程中只介绍了最基本的语法知识
> 在实际用Python的时候发现自己掌握的语法知识远远不够，现在开始看 Python 核心编程（第二版），希望对Python有个更深入的理解。

### 第二章

##### 乘方操作符
	```python
	2 ** 3 = 8
	```
##### for循环遍历  

发现有的人喜欢在遍历列表的时候，不仅遍历列表的元素，同时还得到相应的索引，这时就可以使用 enumerate函数

	```python
	for i,ch in enumerate(foo):
		print ch,'(%d)'%i
	```
	结果：
	```python
	a(0)
	b(1)
	c(2)
	```
##### 错误和异常

```
try:
	fielname = 'test.txt'
	fobj = open(filename,'r')
	fobj.close()
except IOError,e:
	print 'file open error',e
```

##### 函数的默认参数

```
def foo(debug = True):
	if debug:
		print 'Hello World'
	print 'done'
```

### 第三章

##### 同一行书写多个语句

```python
import sys; x = 'foo' ;
```

##### 增量赋值

但是不支持 x++ 或 ++x 这种前置/后置 自增/自减 运算符

```
x += 1
```

##### 多重赋值

```python
x = y = z = 1 # 结果就是 x = 1，y = 1，z = 1
```

##### 多元赋值

```python
x,y,z = 1,2,'string'
x,y = y,x # 方便地交换变量值
```

##### 缩进

缩进的上下边界为 3 个空格 和 7 个空格

##### 模块结构和布局

每个 python 脚本文档应该有清晰的结构，推荐结构如下：
1. 起始行，通常只在类 Unix 环境下使用，有起始行就能够仅输入脚本名字来执行脚本，无需直接调用解释器。
2. 模块文档，介绍模块的功能和重要全局变量的含义，模块外可以通过 module.__doc__ 来访问，使用 help() 查阅模块功能的时候，也是显示的这些内容。
	```python
	"this is a test module"
	```
3. 模块导入语句
4. 变量定义
5. 类定义语句，类功能描述和模块功能描述相似，通过 class.__doc__ 访问
6. 函数定义语句，函数功能描述和模块功能描述相似，通过 function.__doc__ 访问
7. 主程序

##### __name__ == '__main__'

一般在每个模块文件的主程序位置都要加这样一个判断，表示只有该文件作为主程序运行时才会执行以下代码，否则作为模块导入文件的时候是不会执行的。

我们完成了每个模块之后可以在这个位置书写模块的测试代码。

##### 垃圾收集

python 和 java 类似，也是全权负责内存的管理，其中垃圾内存管理，使用变量的引用计数来作为指标，引用次数不大于 0，那么就将变量所占内存回收。
* 增加引用计数
```
x = 3.14
y = x # x 的引用次数加 1
```
* 减少引用次数
	* 本地引用离开了作用范围
	* 对象的一个别名被赋值给其他对象
	* 对象被从一个窗口中移除  
		```python
		myList.remove(x)
		```
	* 窗口对象被删除
		```python
		del myList # myList中的子元素跟着被删除
		```
	* 对象被显式地删除  
		```python
		del x
		```
* 循环引用：使用循环垃圾收集器

```python
x = 123
y = x
x = y # x 和 y 发生了循环引用，x 和 y 的引用次数无论如何最少为 1
```

##### 为什么 Pyhthon 中不需要声明函数类型？

？？？


### 第四章 Python 对象

##### 字符串比较

按照序列值，其实也就是 ASCII 值比较

##### is 和 == 的区别

实际上 a is b 操作等价于 id(a) == id(b)
```python
a = 1
b = 1.0 
a == b # True
a is b # False
```

##### type() 和 isinstance()

type()函数返回任意Python对象的类型,isinstance() 函数判断该对象是何种类型，书上举了这样的例子来说明 type() 和 isinstance()的用法。

```python
num = -69
if isinstance(num,(int,long,float,complex)):
	print type(num).__name__		# 输出 int
num = -5.2+1.9j
if isinstance(num,(int,long,float,complex)):
	print type(num).__name__		# 输出 complex
```
type() 和 isinatance()帮助编程者保证调用的就是期望的对象或函数。

##### 类型工厂函数

类似于 int()、type()、list()这些内置转换函数，调用这些函数的时候实际上是生成了该类的一个实例。
除了常见的之外，支持工厂函数的还有
```python
frozenset()
classmethod()
staticmethod(0
super()
property()
file()
```

##### 标准类型的分类

* 存储模型，分为容器类型（列表，元组等），原子模型（数值，字符串），主要区别就是是否可以包含多个值
* 更新模型，数值和字符串在被更新了之后，id(变量)的值会发生变化，即生成了一个新对象，而列表这种对象在被修改前后，id(变量)的值不变
* 访问模型，直接存取（非容器类型），顺序（列表），映射（字典）。

##### 不支持的类型

* char 或 byte，可以用长度为 1 的字符串来表示
* 指针，python中没有必要使用指针，可以通过id()得到类似于地址的值，但是无法修改
* int short long，Python中的整形实现相当于 c 语言中的长整型
* float vs double，python的浮点类型是 c 语言中的双精度浮点类型，对于精度要求比较高的，可以用 decimal 类型

### 第五章 数字

##### int(),round(),floor()区别

* int: 直接去除小数部分
* round : 四舍五入
* floor : 找到不大于该数字的最小整数

##### // 和 /

* //：地板除
* / : 2.7之后的版本全都是真除法，2.7之前的，对float类型是真除法，对整形是地板除

##### ord() 和 chr()

* ord(): 获取 ASCII 码值
* chr(): 获取 ASCII 码值对应的符号
> 做笔试题的时候可能会用到


### 第六章 序列：字符串、列表和元组

##### 连接操作符 +

##### 重复操作 *

##### 切片操作

```python
sequence[index]
```
* index <0：表示以序列的结束为起点  
	```python
	ns = [1,2,3]
	print ns[-2]   # 输出 2
	```
* 返回一系列值
	```python
	sequence[starting_index : ending_index]
	```
	如果 starting_index = 0，starting_index 可以省略
	```python
	sequence[:ending_index]
	```
	同理如果 ending_index = len(sequence)-1,ending_index 也可以省略
	```python
	sequence[2:]
	```
	如果 ending_index < 0，终点就是 len(sequence) + ending_index
	```python
	ns = [1,2,3]
	print ns[:-1] # 输出 1,2
	```
	返回整个序列
	```python
	sequence[:]
	```
* 使用步长
	```python
	sequence[starting_index : ending_index : step]
	```
	翻转操作
	```python
	sequence[::-1] # 省略开始结束索引，步长为 -1
	```
	开始和结束索引可以超过序列的长度，开始索引小于0按0算，结束索引超过最大索引按最大索引算

##### 字符串类型不可变  
	
	修改字符串中的某个字符，不能通过索引的方式来修改，只能通过创建一个新串。
	
	删除字符串中的某个字符也是一样。

##### 编译时字符串连接

可以使用几个字符串连在一起来构建新字符串
```python
foo = "Hello" "World" # "HelloWorld"
```
通过这种方法，可以把长的字符串分成几部分来写，让添加注释更方便
```python
f = urllib.urlopen( 'http://' #protocol
... 'locahost'				  #hostname
... ':8000'					  #port
... '/cgi-bin/friends2.py')    #file
# 其实就是一个完整的url
```

##### python 中所有对象都具有一个字符串表示，通过 rpr()和 tr()得到

print语句会自动为每个对象调用 str()

##### 字符串模板

python相比较于格式化更强的字符串模板

```python
from string import Template
s = Template('There are ${howmany} ${lang} Quotaion Symbols')
print s.substitute(lang = 'Python',howmany=3)
# There are 3 Python Quotaion Symbols
```

##### 原始字符串 r

忽视转义字符
```python
print r'\n' # 输出：\n
```
	
##### zip()

将两个字符串相应位置字符组合成一个
```python
s,t = 'foa','obr'
zip(s,t)
# [('f', 'o'), ('o', 'b'), ('a', 'r')]
```

##### ''' 应对特殊符号，比如 HTML 文本

##### 列表赋值

切片更新列表
```python
a = [1,2,3,4,5]
a[1:3] = a[3:5] # a = [1,4,5,4,5]
```

##### 删除列表元素

已知索引
```python
del aList[1]
```
不知索引，只知元素
```python
aList.remove('hh')
# 如果有重名元素，那么按顺序删除第一个
```
已知索引，删除并返回该被删除对象
```python
aList.pop()
```

##### 连接操作符 +
```python
a = a + b
```
 '+' 操作符是将 a,b 两个列表重新组合成一个新的列表，然后赋值给 a，a 已经不再是原来的那个列表
```python
a.extend(b)
```
使用extend(),将 b 添加到了 a 的尾部，a 没有发生变化

##### 列表解析

```python
[ i*2 for i in [1,2,3] # 输出 [1,4,9]
```
还可以带有判断
```python
[i for i in range(8) if i%2 == 0] # 输出 [0,2,4,6]
```

##### 内建函数之 sorted() 和 reversed()

返回一个新的列表，而使用 aList.sort()，是就地排序

##### 列表内建函数

```python
list.count(obj) # 返回对象 obj 在列表中出现的次数
list.index(obj,i,j) # 在列表索引i 到 j之间，返回 list[k] = obj 的元素的索引 k
list.insert(index,obj) # 向列表的 index 处添加元素 obj
list.sort(func=None,key = None,reverse = False) # func是比较函数，key是func的参数，reverse = True，递减
```

##### 使用列表构建栈和队列

* 构建栈  
	
	```python
	aList.pop() # 出栈
	aList.append() # 入栈
	```
* 构建队列  
	
	```python
	aList.pop(0) # 出队
	aList.append(0) # 入队
	```

##### 元组的不可变性

元组不可变性和字符串的不可变性类似。

虽然元组元素(指id值)不可变，但是元组中的元素是可变对象的，还是可以改变元素的所包含的元素，例如
```python
t = (['xyz',300],23,-102) # ['xyz',300]中的元素是可变的
```

##### 单元素元组

```python
a = ('xyz',) # 如果没有 ","，a 会被赋值成一个 "xyz" 字符串
```

##### 拷贝 Python 对象，浅拷贝和深拷贝

* 浅拷贝  
	当创建了一个对象 a，并将 a 赋值给了 b，只是将 a 这个引用复制给了 b，修改 b 引用的对象时，也就是在修改 a 引用的对象。
* 深拷贝  
	使用 copy 模块的 copy.deepcopy()
	```python
	import copy
	b = copy.deepcopy(a) # 深拷贝
	b = copy.copy(a) 	 # 浅拷贝
	```
    
### 第七章 字典

##### 创建字典

```python
dict2 = {'name':'earth','port':80}
fdict = dict((['x',1],['y',2]))  # {'y': 2, 'x': 1}
```
从 Python2.3 开始可以用fromkeys()，来创建所有键值相同的字典
```python
ddict = {}.fromkeys(('x','y'),-1) # {'y': -1, 'x': -1}
# 当然第二个参数是可选的，如果不设置，则默认将所有键值设置为 None
```

##### 遍历字典

从Python2.2开始，不必在使用keys()方法先获得键值列表，直接使用迭代器访问
```python
for key in dict2:
    print key,dict2[key]
```

##### 判断字典中是某个键

has_key()方法（不推荐，将被放弃），使用 in 和 not in 操作符。

其中因为字典的键的不可变性，因此列表和其他字典不可以作为键

##### 删除字典中的元素

```python
del dict2['name'] # 删除键为'name'的条目
dict2.clear()     # 删除 dict2 中所有的条目
del dict2         # 删除整个 dict2
dict2.pop('name') # 删除并返回键为 'name' 的条目
```

##### 字典相比于列表

不支持拼接 和 重复


##### 字典的比较

字典的长度——> 字典的键 ——> 字典的值