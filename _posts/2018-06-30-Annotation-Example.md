---
layout:     post
title:      "Annotation-Example"
subtitle:   " \"注解\""
date:       2018-06-16 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Java
---

> java 注解

ActionListenerFor 注解接口

注解相当于定义了一种规范的操作，大部分注解所修饰的方法或者是类在编译代码的时候，通过反射和代理机制，将对注解其修饰的方法或者类做进行相应的操作。

下面举一个 Java核心技术 2 中在讲注解的时候提供的一个例子。这个例子主要实现了为 JFrame 上的按钮绑定监听函数的功能。 一共分为四部分，注解接口，JFrame 窗口，监听函数装载类，主函数。

注解接口，其中只包含一个 source()，用来存储一个字符串值，在本例中该字符串用来存储 JFrame 上的按钮成员名。
```java
package com.edu.bupt.twentyFourth;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.METHOD) // 注解类型，方法
@Retention(RetentionPolicy.RUNTIME) // 只有运行期间有效
public @interface ActionListenerFor {
	String source(); // 注解接口中的属性，存储 String 值
}
```

JFrame 窗口，上面包含三个按钮，分别为yellow Button, blue Button, red Button，三个按钮，点对应的按钮，窗体的背景就会变成对应的颜色。在其构造函数中调用监听函数装载类的装载方法，ActionListenerInstaller.processAnnotations(this);

```java
package com.edu.bupt.twentyFourth;

import java.awt.Color;
import java.lang.reflect.InvocationTargetException;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JPanel;
// ButtonFrame Frame 窗口类
public class ButtonFrame extends JFrame {
	/**
	 *  只包含三个按钮的窗体，点击对应按钮，窗体背景变成对应颜色
	 */
	private static final long serialVersionUID = 1L;
	private static final int DEFAULT_WIDTH = 300;
	private static final int DEFAULT_HEIGHT = 200;

	private JPanel panel;
	private JButton yellowButton;
	private JButton blueButton;
	private JButton redButton;

	public ButtonFrame() throws NoSuchMethodException, IllegalArgumentException, InvocationTargetException {
		setSize(DEFAULT_WIDTH, DEFAULT_HEIGHT);
		panel = new JPanel();
		add(panel);
		yellowButton = new JButton("Yellow");
		blueButton = new JButton("Blue");
		redButton = new JButton("Red");

		panel.add(yellowButton);
		panel.add(blueButton);
		panel.add(redButton);

		// ActionListenerInstaller 类的 processAnnotations 方法
		// 解析注解
		ActionListenerInstaller.processAnnotations(this);
	}

	@ActionListenerFor(source = "yellowButton") // yellowButton 对应该类的属性，添加监听器
	public void yellowBackground() {
		panel.setBackground(Color.YELLOW);
	}

	@ActionListenerFor(source = "blueButton")
	public void blueBackground() {
		panel.setBackground(Color.BLUE);
	}

	@ActionListenerFor(source = "redButton")
	public void redBackground() {
		panel.setBackground(Color.RED);
	}
}
```


用来装载注解的 ActionListenerInstaller 类， 首先利用 ButtonFrame 的 class 对象，获得每个方法上的注解（这是利用了 java 的反射机制），也就得到了每个方法所对应的属性名，然后使用代理类的方法，实现 ActionListener 类的 addActionListener 接口，将属性和对应的方法绑定在一起。
```java
package com.edu.bupt.twentyFourth;

import java.awt.event.ActionListener;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ActionListenerInstaller {
	public static void processAnnotations(Object obj) throws NoSuchMethodException, IllegalArgumentException, InvocationTargetException {
		Class<?> cl = obj.getClass();
		for(Method m : cl.getDeclaredMethods()) {
			// 获取每个方法 m 的注解对象
			ActionListenerFor a = m.getAnnotation(ActionListenerFor.class);
			if(a != null) {
				try {
					// 注解的 source属性 中所存储的字符串就是需要将该方法添加到的属性域
					Field f = cl.getDeclaredField(a.source());
					f.setAccessible(true);
					addListener(f.get(obj), obj, m);
				} catch (NoSuchFieldException |
						IllegalAccessException e
						) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				} catch (SecurityException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
		}
	}

	public static void addListener(Object source, final Object param, final Method m) throws NoSuchMethodException, SecurityException, IllegalAccessException, IllegalArgumentException, InvocationTargetException {
		InvocationHandler handle = new InvocationHandler() {
			@Override
			public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
				// 代理对象虽然在运行期间继承了接口，但是无法完全实现接口所定义的函数
				// 无论调用代理对象的何种方法，都会经过 invoke 方法，并将方法和参数传进 invoke 方法
				// 在 invoke 函数中可以指定返回哪个函数的调用
				if(method.getName().equals("actionPerformed"))
					return m.invoke(param); // 如果是调用 Action 类的 actionPerformed 方法，返回 m 函数调用
				else
				{
					// 其他情况调用 ss 函数，这里的 ss 函数是用来帮助理解代理类的
					for(Method m : this.getClass().getDeclaredMethods()) {
						if(m.getName().equals("ss"))
							return m.invoke(param);
					}
					return null;
				}

			}
			public void ss() {
				System.out.println("ss()");
			}
		};
		// 使用代理机制在创建一个新类，其实现了 java.awt.event.ActionListener 接口
		Object listener = Proxy.newProxyInstance(null, new Class[] {java.awt.event.ActionListener.class}, handle);
		// 获得 adder 函数
		Method adder = source.getClass().getMethod("addActionListener", ActionListener.class);
		// source 是被调用 adder 函数的对象
		// listener 是参数
		adder.invoke(source, listener);
	}
}
```


主函数
```java
package com.edu.bupt.twentyFourth;

import java.lang.reflect.InvocationTargetException;

public class main {
	public static void main(String[] args) throws Exception{
		ButtonFrame bf = new ButtonFrame();
		bf.setVisible(true);
	}
}
```

实现效果：
![实现效果](https://github.com/shihunyewu/shihunyewu.github.io/blob/master/img/blog_img/Annotation.gif)


