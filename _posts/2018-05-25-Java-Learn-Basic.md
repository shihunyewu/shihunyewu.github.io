---
layout:     post
title:      "Java-Learn"
subtitle:   " \"Java 学习中 —— 基本程序设计结构\""
date:       2018-05-25 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Java
---

## 3 章 Java 的基本程序设计结构

##### 3.3 数据类型

* 整形
	|类型|存储需求|
    |----|------|
    |int| 4字节|
    |short| 2 字节|
    |long|8字节|
    |byte|1字节|

* 浮点型
	|类型|存储需求|
    |----|-----|
    |float|4字节|
    |double|8字节|

* 三种特殊的浮点数值
	* 正无穷大，Double.POSITIVE_INFINITY
	* 负无穷大，Double.NEGATIVE_INFINITY
	* 错误，NaN，使用 Double.isNan(x)来判断 x 是否是 NaN
* char 类型
	* 16 位，两个字节
	* char类型描述了 UTF-16 编码中的一个代码单元
	* 建议，编程中，最好将字符串作为抽象数据类型处理。

* boolean类型
	* 不能和整数值转换

##### 3.4 变量

* 尽管 $ 是合法 java 字符，但是不要在代码中使用，它只用在 java 编译器或其它工具生成的名字中。
* java 中可以将声明尽可能地靠近变量第一次使用的地方

##### 3.5 运算符
* Math.pow(x,y)， 幂运算
* 给出了两种取余的实现 Math.floorMode(12,-5) = 3，而 12%-5 = -2
* 数值转换
	* 结果截断，比如(byte)300 的实际值是 44

* 位运算
	* & 是与运算，和 && 完全不同
	* << 和 >> 分别是左移运算和右移运算
	* \>>> 右移运算，使用 0 来填充高位

##### 3.6 字符串
* String 是不可变字符串，不是字符数组，如果需要修改这个字符串中的某一位置字符，要新建一个 String，使用拼接的方法来修改。
* 判断字符串是否相等，需要使用 equal, "==" 是判断两个变量的位置是否相同
	```java
    	String s1 = "hello";
		String s2 = "hello";
        //下面两个表达式都为 true，是因为 java 对字符串的存储做了优化
		System.out.println(s1 == s2); // true
		System.out.println(s1.equals(s2)); // true
        // 下面两个表达式的值不同，说明了问题
		System.out.println(s1.substring(3) == "lo");// false
		System.out.println(s1.substring(3).equals("lo")); // true
    ```
* 空串和 null
	* null  和 "" 不相同

* StringBuilder
	* 和 String 的区别
    	1. 每次想要添加一部分内容的时候，就用 append 方法即可。不必像 String 新建一个String 对象。
    	2. 能够在指定位置插入字符，字符串，能够删除字符，字符串
    	3. 使用 toString() 方法就能得到一个 String 对象

##### 3.7 输入输出
* 输入
	```java
    Scanner in = new Scanner(System.in);
	String str1 = in.nextLine(); // 读取整行，不忽略空格
    String str2 = in.next(); 	// 读取字符串，遇到空格会被截断，分两次读取
    Console cons = System.console();
    String str3 = cons.readPassword("pwd:"); // 可以不回显
    ```
* 格式化输出
	* System.out.printf() 沿用了 c 语言中的 printf 的格式化方法
	* String.format();   创建一个格式化了的字符串
* 文件输入和输出
就是重新定义流的输入源和流向
```java
	public static void main(String[] args) throws IOException {
		PrintWriter outFile = new PrintWriter("file.txt","UTF-8"); // 输出方向指向 file.txt
		outFile.println("Hello world!"); // 向 file.txt 写入 Hello world
		outFile.close();
		Scanner in = new Scanner(new File("file.txt"),"UTF-8"); // 输入来源为 file.txt
		System.out.println(in.nextLine()); // 从输入源中得到字符串
		in.close();
	}
```

##### 3.8 控制流程
* break 和 continue 跳转到标签，实现类似 goto 功能，标签可以放在任何语句块之前，但是不能跳入语句块中。
	* 可以用于在嵌套有多层循环时，从内层循环跳出最外层循环，如下所示
	```java
    public static void main(String[] args) {
		int i=0,j=0,k=0;
		outer:for(i = 0;i< 10;i++) {
			for(j = 0;j< 10;j++) {
				for(k = 0; k < 10;k++) {
					if(k == 5 && j == 5 && i == 5) {
						// 跳出了最外层循环
						// 而不必一层一层地跳出
						break outer;
					}
				}
			}
		}
		System.out.println("i:"+i+" j:" + j + " k:" + k);
		System.out.println("I have jump from the inner of 3 levels for");
	}
    ```
* switch 语句
	* case 没有 break 时，可能触发多个 case
	* case 标签后的变量可以是 char、byte、short 或 int 的常量表达式，或者是枚举常量，从 java7 之后可以是字符串字面量。

##### 大数值
* BigInteger
	* 需要使用方法来替代基本的算术运算
* BigDecimal

##### 数组
* 创建一个数组
```java
// 两种方式
int[] a = new int[100]; //看起来像是将 a 定义为 一个 int[] 型的变量，便于理解
int a[] = new int[100];
```

* foreach
```java
	for(变量类型 变量名: 集合){ }
```

* 常见的数组 api
	* 数组拷贝， 数组 = Arrays.copyOf(数组，length)
	* 拷贝一段数组，Arrays.copyOfRange(数组，int start, int end);
	* 数组排序, Arrays.sort(数组); // 就地排序
	* 数组查找, Arrays.binarySearch(数组， 元素)
	* 分范围查找, Arrays.binarySearch(数组，元素，int start, int end);
	* 全填充， Arrays.fill(数组，元素)，将数组全部填充成参数元素

* 多维数组
	* java 可以创建不规则数组，例如
	```java
    public static void main(String[] args) {
		// 创建多维不规则数组
		int[][] a = new int[10][]; // 需要指定第一个维度
		for (int i = 0; i < a.length; i++) {
			a[i] = new int[i + 1];
			for (int j = 0; j < i+1; j++) {
				a[i][j] = j+1;
			}
			ArrayTest.printArray(a[i]);
		}
	}
    ```
    输出
    ```java
    1	
    1	2	
    1	2	3	
    1	2	3	4	
    1	2	3	4	5	
    1	2	3	4	5	6	
    1	2	3	4	5	6	7	
    1	2	3	4	5	6	7	8	
    1	2	3	4	5	6	7	8	9	
    1	2	3	4	5	6	7	8	9	10
    ```
