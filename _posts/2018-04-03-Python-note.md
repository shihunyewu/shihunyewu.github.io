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

##### \_\_name\_\_ == '\_\_main\_\_'

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

##### 元组定义时，括号非必须

```python
s = '1','2','3' # 相当于 s = ('1','2','3')
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

##### 映射类型相关的函数

工厂函数被用来创建字典。可以传递一个容器对象作为参数。将容器对象中的组合转化成键和值。

```python
>>> dict(zip(('x', 'y'), (1, 2)))
{'y': 2, 'x': 1}
>>> dict([['x', 1], ['y', 2]])
{'y': 2, 'x': 1}
>>> dict([('xy'[i-1], i) for i in range(1,3)])
{'y': 2, 'x': 1}
```

接受关键字或关键字参数字典

```python
dict(x = 1,y = 2)
{'y':2,'x':1}
```

可以拷贝另一个字典

```python
dict9 = dict(**dict8)
# 或者
dict9 = dict8.copy()
# 二者都是深度拷贝
```

##### 映射类型内建方法

dict.upate(dict2) # 将字典 dict2 的 键-值对添加到字典 dict 中，添加dict中不存在的键值对，将dict和dict2同时存在的键值对，更新成dict2的值

##### 键必须是可哈希的

可变类型不可以做键，比如列表，字典等，属于不变类型的元组就可以做键

##### 集合类型的创建

集合类型只能使用工厂方法 set() 和 frozenset()来创建

##### 更新集合

```python
s.add('z') # 将 z 添加到集合中
s.update('pypi') # 将 'pypi' 更新到集合中，相当于做并
s.remove('z') 	# 删除
s -= set('pypi') # 做差运算
del s # 删除整个集合
```
##### 集合类型操作符

* 联合 | 
	```python
	z = s|t
	# z 包含了 t 和 s 所有的元素
	```
* 交集 &
* 差集 -
* 对称分布，两个集合不属于对方的值的集合，也就是异或操作

##### 混合集合类型操作

如果左边集合是可变集合，右边集合是不可变集合，那么结果是可变结合。位置相反，结果相反。

##### 集合类型操作符（仅用于可变集合）

```python
|=
&=
-=
^=
```

##### 内建函数

* set()和 frozenset()  
	set() 生成可变集合，frozenset() 生成不可变集合。  
	参数必须是可以迭代的，比如字符串，字典，列表，元组


### 第八章 条件和循环

##### 条件语句缩进解决了"悬挂else" 问题

```c
/* dangling-else in C */
if (balance > 0.00)
	if (((balance - amt) > min_bal) && (atm_cashout() == 1))
		printf("Here's your cash; please take all bills.\n");
else
	printf("Your balance is zero or negative.\n");
```
上面的 else 语句会给人一种错觉，好像 else 是属于上面的 if 语句（其实是属于下面的 if）。而 python 的强制缩进就可以避免这种错觉。

##### 使用字典来替代 switch 语句

python 中不支持 switch 语句，但是可以用字典来作为 switch 的替代品。

```python
msgs = {'create': 'create item',
'delete': 'delete item',
'update': 'update item'}
default = 'invalid choice... try again!'
action = msgs.get(user.cmd, default) # 相当于 switch 做选择
```

##### 条件表达式

X if C else Y，其中 X 和 Y 是两个待选的值，如果表达式 C 为 True，返回 X，否则返回 Y

##### for 语句

for语句可以遍历序列成员，可以用在列表解析 和 生成器表达式中，它会自动调用迭代器的next() 方法。其实其更像 shell 语句中的 foreach	

##### for 用于迭代器类型

迭代对象有一个 next() 方法，每次调用之后返回下一个条目，所有条目迭代完之后，迭代器引发 StopIteration 异常，终止循环，for 语句在内部调用 next() 并捕获异常。

##### range()

range(start,end,step = 1)，返回一个列表。每个列表元素 k 都满足，start \<= k \< end.

range(start = 0,end, step = 1) # 报错，range() 不可以用过两个参数调用

实际上，为了运行速度，python2.x 实现了一个 迭代器对象 xrange()，其只在 for 循环中有效，其不返回列表。在 python3.x 中，range() 替代了 xrange()，并删除了 xrange()这个函数。

##### 与序列相关的内建函数

sorted(),reversed().enumerate(),zip()，其中 sorted() 和 zip() 返回一个序列，另外两个函数返回一个迭代器(就是一个具有 next() 方法的对象，具有触发循环终止的能力)

##### else 可以放到 while，for 之后

##### 迭代器和 iter() 函数

* 迭代器如何迭代？  
	迭代器又一个 next() 方法的对象，而不是通过索引来计数。每次调用 next() 方法返回下一个元素。全部的条目迭代结束之后，会引发一个 StopIteration，表示迭代完成。

* 迭代器的限制
	* 不能向前移动
	* 不能复制一个迭代器，如果想要再次迭代同个对象，需要重新建一个迭代器对象
	
* 使用迭代器

```python
>>> myTuple = (123, 'xyz', 45.67)
>>> i = iter(myTuple)
>>> i.next()
123
>>> i.next()
'xyz'
>>> i.next()
45.67
>>> i.next()
Traceback (most recent call last):
File "", line 1, in ?
StopIteration
```

```python
for i in seq:
	do_something_to(i)
# 实际上是这样工作的:
fetch = iter(seq)
while True:
	try:
		i = fetch.next()
	except StopIteration:
```

##### 可变对象和迭代器

在迭代可变对象时，修改迭代对象时不可取的。而迭代器就能够避免这种问题，一个序列的迭代器只记录当前到达的元素，如果在迭代时改变了元素，会报错。

##### 创建迭代器

iter(obj): obj 是一个序列

iter(func,sentinel),用类来创建迭代器，重复调用func，直到迭代器下一个值为 sentinel

##### 列表解析

\[expr for iter_var in iterable\],其中 expr 是一个表达式

\[expr for iter_var in iterable if cond_expr \],带 if 语句的 for 循环

##### 生成器表达式 

\(expr for iter_var in iterable if cond_expr\)

它与列表解析非常相似，而且它们的基本语法基本相同;不过它并不真正创建数字列表,而是返回一个生成器，这个生成器在每次计算出一个条目后，把这个条目“产生”(yield)出来.生成器表达式使用了"延迟计算"(lazy evaluation)

用到的时候，才会生成,举一个例子

```python
>>> s = [1,2,3]
>>> t = [4,5,6]
>>> x = ((i,j) for i in s for j in t)
>>> for item in x:
...     print(item)
...
(1, 4)
(1, 5)
(1, 6)
(2, 4)
(2, 5)
(2, 6)
(3, 4)
(3, 5)
(3, 6)
```

**生成器没有全部元素，只是每次计算下一个结果**


### 第九章 文件和输入输出

##### 工厂函数file()

该函数和 open()效果相同，在想说明处理文件对象的时候使用 file(),让程序的语意更清楚明白

##### 通用换行符支持(UNS)

不同平台来表示行结束的符号是不同的，例如 \n,\r,或者 \r\n.当使用 rU 方式打开文件的时候，python 会将所有的行分隔符替换成 NEWLINE(\n)。这样按行读取的时候，兼容各种换行符。

##### 处理行结束符

使用read() 或 readlines() 时，Python 不会删除行结束符，如果不做任何操作，字符串中多一个换行符，像这样：
```python
One day

Two day
```
推荐使用strip()方法去除首尾的空格和换行符(参数为空，默认去除空格和换行符，不为空时，去除首尾的参数，如 strip('0')，去除首尾的 '0')

```python
f = open('myFile','r')
data = (line.strip() for line in f.readlines())
for line in data:
	print(line) # 行和行之间不会多一个换行符
f.close()
```

##### 文件内移动

seek() 和 C 语言中的 fseek()类似，有两个参数 offset(偏移量) 和 whence(从哪里开始，0 从文件开头，1 从当前位置，2 从结尾)

##### 文件迭代

因为新版本 python 中的 file 对象集成了 next() 方法，因此在按行输出的时候，可以直接这样写

```python
for line in f: # 而不必像以前的版本那样， for line in f.readlines()，二者效果相同
	print(line.strip()) 
```

##### 文件对象的垃圾回收机制

Python 垃圾收集机制也会在文件对象的引用计数将为 0 的时候，自动关闭文件。建议在用这个文件来赋值另一个文件对象之前关闭这个文件，如果不显式地关闭文件，很可能丢失输出缓存区的数据。

可以使用 flush() 方法，直接将内部缓冲区中的数据立刻写入到文件中。

##### 文件方法杂项

print 方法默认给输出字符串的末尾添加了 '\\n'，如果想不输出这个换行符，可以
```python
print(s,) # 在参数最后添加一个 ,
```

不同平台的行分隔符不同，python 只要导入了 os 模块，以下这些变量都会被正确赋值

```python
linesep 用于在文件中分隔行的字符串
sep 用来分隔文件路径名的字符串
pathsep 用于分隔文件路径的字符串
curdir 当前工作目录的字符串名称
pardir (当前工作目录的)父目录字符串名称
```

##### truncate([size]) 截断文件

默认从当前截断，输入 size 之后，将文件的前 size 部分截断，即删除 size 之后的部分

##### 标准文件

通过 sys模块 来访问三个标准文件 sys.stdin（标准输入，一般是键盘）,sys.stdout（标准输出，一般为到显示器的缓冲输出） 和 sys.stderr（标准错误，到屏幕的非缓冲输出），print 语句通常输出到 sys.stdout,raw_input() 通常输入到 sys.stdin。

？？需要自己处理好换行符。。。

##### 从 shell 中接收参数

sys.argv，接收参数数组，python 中不设置 sys.argc （即参数个数）

##### 文件系统

有时候会使用 python 来管理一下文件，这时候会用到 os 这个模块的函数。

* remove()/unlink 删除文件
* rename() 重命名
* walk() 找到目录树下的所有文件名，相当于递归遍历
* chdir() 改变工作目录
* listdir() 列出指定目录下的文件
* getcwd()  返回当前工作目录
* mkdir()/mkdirs()  创建目录/创建多层目录
* rmdir() / removedirs() 删除目录/删除多层目录，只对空目录有效，当目录不为空时，采用 shutil.rmtree()来删除，或者配合 walk() 来删除，效果相同。

os.path 模块中的路径访问函数

* basename() 去掉目录路径，返回文件名，做爬虫时，用过
* dirname() 去掉文件名，返回目录路径
* join() 将分离的各部分组合成一个路径名
* split() 返回(dirname(),basename())元组
* splitdrive() 返回(dirvename,pathname) 元组，就是目录名和文件名
* getsize() 返回文件的大小
* exists() 指定路径是否存在
* isabs() 路径是否为绝对路径
* isdir() 指定路劲是否为目录
* isfile 指定路径是否为文件
* islink() 是否为符号链接
* samefile() 是否指向同一个文件

##### 永久存储

python 真是服务周到，当编程者想保留一些信息的时候，又不想使用关系数据库管理系统，python 提供了在这种情景下使用的简单的永久性存储模块。当然，python 内置了不需要一个单独的服务器进程或操作的系统，并且不需要配置的关系数据库 SQLite，
某些情况可以考虑使用 SQLite。

* pickle 和 marshal 模块

pickle 模块依托文本文件，使用二进制写入和读出方式保存的对象。下面是小例子：

```python
>>> import pickle
>>> class Person:
...     def __init__(self,n,a):
...             self.name = n
...             self.age = a
...     def show(self):
...             print(self.name,str(self.age))
...
>>> p1 = Person('bob',2)
>>> p1.show()
bob 2
>>> f = open('p.txt','wb')	# 使用二进制写入方式
>>> pickle.dump(p1,f,0)		# 将对象写入
>>> f.close()
>>> f = open('p.txt','rb')	# 使用二进制方式读
>>> bb = pickle.load(f)		# 装载对象
>>> bb.show()				# 执行对象的 show()
bob 2
>>> f.close()
```

marshal 用法和 pickle 类似


### 第十章 错误和异常

##### 检测异常

try-except 语句和 try-finally 语句，或者是二者的复合语句，举个列子
```python
>>> try:
...     f = open('ball','r')
... except IOError as e: # 2.X版本支持 except IOError ,e: 这种写法，但是 3.X 不支持
...     print(e)
...
[Errno 2] No such file or directory: 'ball'
```

##### 同时处理多个异常

```python
try:
    client_obj.get_url(url)
except (URLError, ValueError, SocketTimeout):
    client_obj.remove_url(url)
```

##### 异常存在层级关系

可以用基类来捕获所有异常，如
```python
try:
    f = open(filename)
except (FileNotFoundError, PermissionError):
    pass
```

可以被重写为
```python
try:
    f = open(filename)
except OSError:
    pass
```
因为 OSError 是 FileNotFoundError 和 PermissionError 异常的基类

##### 捕获多个异常捕获

```python
try:
    f = open('hh', 'r')
except (FileNotFoundError, ValueError) as e: # 只捕获其中的一个异常就会执行下面的操作，所以二者只用一个 as e 就行了
    print(e)
```

##### 可以基于状态码来做处理

用更一般的异常类型的状态码来做更详细的区分

```python
import errno

filename = 'hh'
try:
    f = open(filename)
except OSError as e:
    if e.errno == errno.ENOENT:
        print('File not found')
    elif e.errno == errno.EACCES:
        print('Permission denied')
    else:
        print('Unexpected error: %d', e.errno)
```

##### 特别的异常优先

当匹配多个异常时，尽量让更一般的错误在最后，如果更一般的异常包含了特殊性的异常，更一般的异常会首先匹配，这样后面的特殊性的异常就不会被匹配了。比如这样
```python
>>> try:
...     f = open('missing')
... except OSError:
...     print('It failed')
... except FileNotFoundError: # 不会被匹配，因为 OSError 包含了 FileNotFoundError
...     print('File not found')
```

##### 所有异常的基类 Exception 捕获所有的异常

##### 自定义异常

创建自定义异常，让类来继承 Exception ,并书写判断条件，使用 raise 手动触发异常

```python
class RunError(Exception):
    def __init__(self, message, status):
        super().__init__(message, status)
        self.message = message
        self.status = status


try:
    i = 0
    while True:
        i += 1
        if i > 100:
            raise RunError(u'i 超过了 100', 0)  # 使用 raise 来触发自定义异常
except RunError as e:
    print(e.args) # 输出 ('i 超过了 100', 0) 异常将参数添加到 e.args 中
```

？？是否可以自动触发自定义异常

##### python 没有往上层抛出异常

##### else 子句

没有检测到异常的时候，会运行 else 子句

##### finally 子句

无论异常是否发生，都会执行这段代码

##### with 语句

Python 对一些内建对象进行改进，加入了对上下文管理器的支持，可以用于 with 语句中，比如可以自动关闭文件、线程锁的自动获取和释放等。

```python
with open(r'somefileName') as somefile: 
    for line in somefile:
        print line 
        # ...more code
```

上面这段代码相当于用下面的 try/finally 方式实现，with 只是实现了 finally 语句，没有捕获异常，如果有异常，运行时依旧会抛出。

```python
somefile = open(r'somefileName')
try:
    for line in somefile:
        print line
        # ...more code
finally:
    somefile.close()
```

已经加入对上下文管理协议支持的还有模块 threading、decimal 等

文管理器要实现上下文管理协议所需要的 __enter__() 和 __exit__() 两个方法：
[自定义上下文管理器](https://www.ibm.com/developerworks/cn/opensource/os-cn-pythonwith/)

##### assert 断言

assert 需要用 AssertionError 来捕获

```python
try:
	assert 1==0
except AssertionError:
	print('不相等')
```

##### logging

日志

```python
import logging
logging.basicConfig(level=logging.INFO) # 可以配置日志等级，有DEBUG，INFO，WARNING，ERROR 等级别
logging.info('n = %d' % n)
```

##### pdb 调试器

pdb 调试器类似于 gdb 调试器


### 第十一章 函数和函数式编程



































