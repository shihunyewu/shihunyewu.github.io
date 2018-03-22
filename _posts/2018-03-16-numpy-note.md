---
layout:     post
title:      "Numpy-Pandas-Matplotlib-note"
subtitle:   " \"Numpy-Pandas-Matplotlib\""
date:       2018-03-16 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-alitrip.jpg"
catalog: false
tags:
    - ML
---

> "Numpy-Pandas-Matplotlib 笔记"

参加kaggle比赛，学会使用numpy(基本数学运算库)，pandas（用于数据处理的库），matplotlib（用于绘图的库），sklearn（基本的机器学习库）这几个库会节省很多的精力。

#### numpy基础

* 数组的常见属性
	+ ndarray.ndim：  
	数组的维度
	+ ndarray.shape:  
	数组的尺寸
	+ ndarray.dtype:  
	数组中元素的数据类型
	+ ndarray.itemsize:  
	数组元素的大小
* 创建一个数组
	* 创建一个普通数组
	```python
	# 创建一个普通数组
	from numpy import *
	a = array([2,3,4])
	```
	* 创建一个复数数组
	```python
	from numpy import *
	a = array([[1,2],[3,4]],dtype = complex)
	```
	```python
	array([[ 1.+0.j,  2.+0.j],
		   [ 3.+0.j,  4.+0.j]])
	```
	
	* 创建零数组
	```python
	zeros((3,4)) # 创建一个3*4的零数组
	```
	* 创建全 1 数组
	```python
	ones((3,4),dtype = int16)
	```
	* 创建全空数组
	```python
	empty((2,3))
	```
	* 创建连续数组
	```python
	arange(10,30,5)# 表示10到30，步长为5
	array([10,15,20,25,30])
	```
	* 创建等分数组
	```python
	linspace(0,2,9) # 表示将[0,2]这个区间 9 等分
	array([ 0.  ,  0.25,  0.5 ,  0.75,  1.  ,  1.25,  1.5 ,  1.75,  2.  ])
	```
* 打印数组
```python
a = arange(6)
print arange
[0 1 2 3 4 5]
```

* 数组的基础操作
	* 加减乘除，平方，使用内置函数
	```python
	a = array([20,30,40,50])
	b = arange(4)
	c = a-b # 两个数组相减
	d = a+b # 两个数组相加
	e = b**2 # 数组中所有的元素都做平方
	f = b*2
	g = 10 * sin(a)
	a < 35
	array([True, True, False, False], dtype=bool)
	```

	* 矩阵操作，矩阵元素乘，点乘，相加，相减
	```python
	A = array( [[1,1],
	...          [0,1]] )
	B = array( [[2,0],
	...           [3,4]] )
	```
	```python
	A*B   # 元素乘
	array([[2, 0],
       [0, 4]])
	dot(a,b) # 矩阵乘
	array([[5, 4],
       [3, 4]])
	```

	* 对数组求和，求最小值，求最大值
		* a.sum()
		* a.min()
		* a.max()
		* a.sum(axis = 0) # 对每一列求和
		* a.sum(axis = 1) # 对每一行求和
		* a.cumsum(axis = 1)
		  ```python
			array([[ 0,  1,  3,  6],
					[ 4,  9, 15, 22],
					[ 8, 17, 27, 38]])
		  ```
* 通用的函数
	```python
		b = arange(3)
	```
	* exp(b)
	* sqrt(b)
* python数组的索引，切片，迭代都满足
	* 简略写法
	```python
	x[1,2,...] is equivalent to x[1,2,:,:,:],
	x[...,3] to x[:,:,:,:,3] and
	x[4,...,5,:] to x[4,:,:,5,:].
	```
	* 可以整行遍历
	```python
	for row in b:
		print row
	```
	* 可以将一个多维度的数组展平
	```python
	for ele in b.flat:
		print ele
	```
* 尺寸变换
	* a.reshape(3,4) 和 a.resize(3,4)的区别：  
		reshape是返回一个新的数组，resize是在原地变换
	* a.reshape(3,-1)  
		表示第二维自动计算  

[未完待续](http://scipy.github.io/old-wiki/pages/Tentative_NumPy_Tutorial)

#### pandas

1. pandas的两种数据结构，Series 和 DataFrames
	* Series是一种一维对象，类似数组列表或者是表格中的一列
		* 创建：
			* 每个元素可以是任何元素
			```python
			# create a Series with an arbitrary list
			s = pd.Series([7, 'Heisenberg', 3.14, -1789710578, 'Happy Eating!'])
			s
			```
			0                7
			1       Heisenberg
			2             3.14
			3      -1789710578
			4    Happy Eating!
			dtype: object
			* 或者是给每个元素自定义一个索引
			```python
			s = pd.Series([7, 'Heisenberg', 3.14, -1789710578, 'Happy Eating!'],
              index=['A', 'Z', 'C', 'Y', 'E'])
			```
			* 可以从由字典转换而来，字典的键就成了Series的索引
			```python
			d = {'Chicago': 1000, 'New York': 1300, 'Portland': 900, 'San Francisco': 1100,
			 'Austin': 450, 'Boston': None}
			cities = pd.Series(d)
			cities
			```
	* DataFrames  
	DataFrames是一种二维表格，行列组成。当然也可以将其想象成是很多个共享索引的Series对象
		* 和csv数据的交互（xlsl类似）
		```python
		from_csv = pd.read_csv('mariano-rivera.csv') 
		cols = ['num', 'game', 'date', 'team', 'home_away', 'opponent',
        'result', 'quarter', 'distance', 'receiver', 'score_before',
        'score_after']
		no_headers = pd.read_csv('peyton-passing-TDs-2012.csv', sep=',', header=None,
                         names=cols)
		# 遇到没有指定列名的数据，可以在读之前指定列名
		```
		``` python
			my_dataframe.to_csv('path_to_file.csv')s # 导出成.csv文件
		```
		
		* 使用DataFrame
			* movies.info()：显示读入的数据的信息
			```
			<class 'pandas.core.frame.DataFrame'>
			Int64Index: 1682 entries, 0 to 1681
			Data columns (total 5 columns):
			movie_id              1682 non-null int64
			title                 1682 non-null object
			release_date          1681 non-null object
			video_release_date    0 non-null float64
			imdb_url              1679 non-null object
			dtypes: float64(1), int64(1), object(3)
			memory usage: 78.8+ KB
			```
			
			这些输出主要说明一下几点：
			
				1. 这确实是一个DataFrame实例
				2. 行的索引从 0 - (N-1)，N是数据的总行数
				3. 一共有 1682 个实例对象
				4. 数据有5列
				5. 每一列的数据类型
				6. 这些数据所占内存
			* users.describe():描述数据特征
			```python
				       user_id	    age
				count	943.000000	943.000000
				mean	472.000000	34.051962
				std	272.364951	12.192740
				min	1.000000	7.000000
				25%	236.500000	25.000000
				50%	472.000000	31.000000
				75%	707.500000	43.000000
				max	943.000000	73.000000
			```
			
			* pandas.DataFrame.plot
				* 参数
				1. data
				2. x
				3. y
				4. kind:
					* 'line' 线性图
					* 'bar' 柱状图
					* 'barth' 横向柱状图
					* 'hist' 直方图
					* 'kde' 曲线图
					* 'pie' 饼状图
					* 'scatter' 散点图

[pandas's api](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html#pandas.DataFrame)  
[未完待续](http://www.gregreda.com/2013/10/26/working-with-pandas-dataframes/)	
	

