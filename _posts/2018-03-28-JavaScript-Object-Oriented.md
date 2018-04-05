---
layout:     post
title:      "JavaScript-Object-Oriented"
subtitle:   " \"js-OO\""
date:       2018-03-28 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - JavaScript
---

> "JavaScript中的面向对象程序设计"，笔记

JavaScript语法中没有类的概念，因此它的对象也与基于类的语言中的对象有所不同。

#### 如何创建对象

##### 工厂模式

用函数来封装接口创建对象的细节

```python
function createPerson(name, age, job){
	var o = new Object();
	o.name = name;
	o.age = age;
	o.job = job;
	o.sayName = function(){
	alert(this.name);
};
	return o;
}
var person1 = createPerson("Nicholas", 29, "Software Engineer");
var person2 = createPerson("Greg", 27, "Doctor");
```
* 存在的问题：  
	无法得知一个对象是何种类型，解决方案——构造函数模式

##### 构造函数模式

```python
function Person(name, age, job){
	this.name = name;
	this.age = age;
	this.job = job;
	this.sayName = function(){
		alert(this.name);
	};
}
var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");
```

这样使用对象的 constructor 属性来判断是否是 Person 对象。

```python
alert(person1.constructor == Person); //true
alert(person2.constructor == Person); //true
```

* 存在的问题：  
	person1和person2中均含有一个sayName函数，两个对象的sayName函数并不相同。其他的语言中所有实例的函数共用一个模板，否则浪费资源，所以此处应该做一个改进。

```python
function Person(name, age, job){
	this.name = name;
	this.age = age;
	this.job = job;
	this.sayName = sayName;
}
function sayName(){
	alert(this.name);
}
var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");
```

上面代码将该类对象共有的方法，放到了global环境下，这样所有实例的这个函数只有一个模板。
* 存在的问题：  
	这样就破坏了对象的封装性，该问题可以使用原型模式来解决。

##### 原型模式

每个函数都有一个prototype属性，该属性是一个指针，指向一个对象。这个对象让所有的对象实例共享它所包含的属性和方法。实现如下：

```python
function Person(){}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
	alert(this.name);
};
var person1 = new Person();
person1.sayName(); //"Nicholas"
var person2 = new Person();
person2.sayName(); //"Nicholas"
alert(person1.sayName == person2.sayName); //true
```






