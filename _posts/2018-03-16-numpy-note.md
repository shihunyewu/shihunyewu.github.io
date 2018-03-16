---
layout:     post
title:      "Numpy-Pandas-Matplotlib-note"
subtitle:   " \"Numpy-Pandas-Matplotlib\""
date:       2018-03-16 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-2015.jpg"
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
	
	

