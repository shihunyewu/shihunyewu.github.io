---
layout:     post
title:      "Flask"
subtitle:   " \"web 框架学习中\""
date:       2018-05-01 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Python
---

> 一本好书可以带你走进一个新世界，一本坏的文档，会让你想学习的欲望一扫而空。比如[欢迎使用Flask](http://docs.jinkan.org/docs/flask/)，看了之后，我根本感受不到它对我的欢迎。。。最终还是换了一本书——《FlaskWeb开发》

### Flask 学习笔记

### 第 2 章 程序的基本结构

##### 2.1 初始化

所有的 Flask 程序都必须创建一个程序实例。Web 服务器使用 WSGI 协议。

```python
from flask import Flask
app = Flask(__name__)

```

用 __name__ 作为参数传给 Flask ，Flask 用这个参数决定程序的根目录，便于找到相对于程序根目录的资源文件位置。

##### 2.2 路由和视图函数

##### 2.3 启动服务器

```python
if __name__ == '__main__':
	app.run(debug=True) # 启动开发服务器，但是在生产环境中不会使用这个服务器
```

##### 2.4 一个完整的程序

##### 2.5 请求——响应循环

* 程序和请求上下文

* 请求调度
	Flask 使用app.route 修饰器或者非修饰器形式的app.add_url_rule() 生成映射。

* 请求钩子
	Flask 提供了注册通用函数的功能，注册的函数可在请求被分发到视图函数之前或之后调用。使用装饰器来实现
	
	• before_first_request：注册一个函数，在处理第一个请求之前运行。
	• before_request：注册一个函数，在每次请求之前运行。
	• after_request：注册一个函数，如果没有未处理的异常抛出，在每次请求之后运行。
	• teardown_request：注册一个函数，即使有未处理的异常抛出，也在每次请求之后运行。

* 响应

```python
from flask import make_response
@app.route('/')
def index():
	response = make_response('<h1>This document carries a cookie!</h1>')
	response.set_cookie('answer', '42') # 可以设置 cookie
	return response
```

可以重定向到其他链接
```python
from flask import redirect
@app.route('/')
def index():
	return redirect('http://www.example.com')
```

还有一种特殊的响应由 abort 函数生成。

```python
from flask import abort
@app.route('/user/<id>')
def get_user(id):
	user = load_user(id)
	if not user:
	abort(404)
	return '<h1>Hello, %s</h1>' % user.name
```
abort 不会讲控制权还给调用它的函数，而是抛出异常，将控制权交给服务器

##### 2.6 Flask扩展

### 第 3 章 模板

##### 3.1 Jinja2 模板引擎

* 3.1.1 渲染模板

* 3.1.2 变量
模板中使用 {{name}} 结构表示一个变量，jinja2 能够识别所有类型的变量，像列表、字典和对象。

	* 可以使用过滤器修改变量，比如
	```html
	<!-- 这里的 captitalize 是将值的首字母转换成大写，其他字母转换成小写字母 -->
	Hello,{{name | capitalize}}
	```
	还有很多的可选操作
* 3.1.3 控制结构

	* 条件控制语句
	```html
	{% if user %}
		<li>{{comment}} </li>
	{% else %}
		Hello,Stranger!
	{% endif %}
	```

	* 循环语句
	```html
	{% for	comment in comments %}
		<li>{{comment}} </li>
	{% endfor %}
	```

	* 支持宏
	宏类似于函数
	```html
	{% #这里的 macro 是定义宏的关键字？%}
	{% macro render_comment(comment) %}
		<li>{{ comment }}</li>
	{% endmacro %}
	```

	* 类似于模块调用

	```python
	{% #macros.html%}
	{% macro render_comment(comment) %}
		<li>{{ comment }}</li>
	{% endmacro %}
	```

	```html
	{% #index.html%}
	<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Hello</title>
	</head>
	<body>
    {%# 改用引用别的模块中的函数%}
	{% import 'macros.html' as macros %}
	<ul>
		{% for comment in comments %}
        	{%# 像引入包一样引入模块，但是调用时要用现在的包名%}
			{{ macros.render_comment(comment) }}
		{% endfor %}
	</ul>

	</body>
	</html>
	```

	* 拼接 html

	```python
	<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Hello</title>
	</head>
	<body>
    {%#  top.html 作为网站的头部 %}
	{% include 'top.html' %}
	{{ word|capitalize }}
	{% if user %}
		Hello,{{ user }}
	{% else %}
		Hello,Stranger!
	{% endif %}
	</body>
	</html>

	```

	* 模板继承

		在子模板的开头定义了”{% extend ‘parent.html’ %}”语句来声明继承，此后在子模板中由”{% block block_name %}”和”{% endblock %}”所包括的语句块，将会替换父模板中同样由”{% block block_name %}”和”{% endblock %}”所包括的部分。

		这就是块的功能，模板语句的替换。这里要注意几个点：

		1. 模板不支持多继承，也就是子模板中定义的块，不可能同时被两个父模板替换。
		2. 模板中不能定义多个同名的块，子模板和父模板都不行，因为这样无法知道要替换哪一个部分的内容。
		
		另外，我们建议在”endblock”关键字后也加上块名，比如”{% endblock block_name %}”。虽然对程序没什么作用，但是当有多个块嵌套时，可读性好很多。
		
		如果父模板中的块里有内容不想被子模板替换怎么办？在 block 结构中使用 super()
		
		[jinja2模板的继承](http://www.bjhee.com/jinja2-block-macro.html)

* 3.3 使用 Flask-Bootstrap 继承 Twitter Bootstrap
	[Bootstrap](http://getbootstrap.com/)
	
	很强，从 Flask-Bootstrap 上继承了很多已经定义好了的 Block 块。


* 3.4 链接
url_for() 函数最简单的是以视图函数名作为参数，返回对应的URL。
```python
url_for('index', page=2) 的返回结果是/\?page=2
```

* 3.5 静态文件
```python
url_for('static', filename='css/styles.css', _external=True) # 参数名一定要是 filename
```

* 3.6 使用Flask-momen本地化日期和时间

##### 第 4 章 Web 表单

* 4.2 表单类 wtf
	Flask-WTF 可以处理 Web 表单。

* 4.3 将表单渲染成 HTML

* 4.4 在视图函数中处理表单

* 4.5 重定向和用户会话

* 4.6 Flash 消息
	* Flash 用来响应用户的请求，给用户返回一个消息
	主要就是 flash() 和 get_flashed_messages()的应用。在app.py中使用 flash()放警告信息，在page中用 get_flashed_messages() 取信息。

##### 第 5 章 数据库

##### 参考文档

《FlaskWeb开发》