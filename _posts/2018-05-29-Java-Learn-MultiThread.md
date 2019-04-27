---
layout:     post
title:      "Java-Learn"
subtitle:   " \"Java 学习中 —— 多线程\""
date:       2018-05-29 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Java
---

> 每次读都应该有新的体会

## 14 章 线程
### 14.2 中断线程

* 默认终止条件：
	* run 方法执行到最后一条语句

* 强制中断线程：
	* interrupt 方法
当对一个线程调用了 interrupt 方法时，线程的中断状态将被置位，每个线程都应该不断地检查这个标志，使用静态的 Thread.currentThread 方法获得当前线程，然后调用 isInterrupted method 方法。
```java
	while(!Thread.currentThread().isInterrupted && more work to do){
    	do more work;
    }
```

###### 处理中断
假设 A 线程为主线程，B 线程为子线程。A 线程调用 B 线程的 interrupt 方法，分为以下两种情况：
1. B 线程内没有 sleep() 或者 wait()
线程内时刻调用 IsInterrupt 方法返回中断信号，判断是否中断，如果中断，做出相应的反应。
```java
package interruptLearn;
public class InterruptTest {
	public static void main(String[] args) {
		TestThread testThread = new TestThread();
		testThread.start();

		// 延时3秒后  interrupt 中断
		try {
			Thread.sleep(3000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		testThread.interrupt();
	}

	private static class TestThread extends Thread {

		@Override
		public void run() {
			int num = 0;
			while (true) {
				if (isInterrupted()) { // 外部中断当前线程后，本线程检测到中断
					System.out.println("当前线程 isInterrupted");
					// 对外部的中断做出响应，跳出本循环
					break;
				}
				num++;
				if (num % 100 == 0) {
					System.out.println("num : " + num);
				}
			}
		}
	}
}
```
2. B 线程存在 sleep() 或者 wait()
处在 sleep() 和 wait() 状态时，外部线程对本线程进行中断时，会抛出 InterruptedException 异常。因此除了 1 中的情况还要在捕获到中断异常之后做对应的处理。另外捕获异常之后，线程 B 的中断状态标志位会被置为 false，因此这就给了我们两个选择，一个是处理好异常之后，接着运行，一个是处理好异常之后，直接跳出任务，或者是自己中断自己，触发本线程别的地方的中断处理程序。
```java
package interruptLearn;
public class InterruptTest {
	public static void main(String[] args) {
		TestThread testThread = new TestThread();
		testThread.start();

		// 延时3秒后  interrupt 中断
		try {
			Thread.sleep(3000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		testThread.interrupt();
	}

	private static class TestThread extends Thread {

		@Override
		public void run() {
			int num = 0;
			while (true) {
				if (isInterrupted()) { // 外部中断当前线程后，本线程检测到中断
					System.out.println("当前线程 isInterrupted");
					// 对外部的中断做出响应，跳出本循环
					break;
				}
				num++;
				try {
					sleep(10);
				} catch (InterruptedException e) {
					// 捕获中断异常
					// 推荐三种方式
					// 一种是不中断当前线程
					// System.out.println("我睡眠的时候有人来中断了");
					// 一种是直接退出本线程
					// break;
					// 一种是自己中断自己，然后触发别的地方的中断处理程序
					// interrupt();
				}
				if (num % 100 == 0) {
					System.out.println("num : " + num);
				}
			}
		}
	}
}
```

#### 四个用到的方法
1. interrupt，向线程发送一个中断。线程的中断标志位将被置为 true，如果线程在 wait 或者是 sleep，将抛出中断异常，并将中断标志位置为 false。
2. static interrupted，测试当前线程是否中断，返回中断标志位的值，并将中断标志位置为 false。
3. boolean isInterrupted，测试当前线程是否被中断，返回中断标志位。
4. Thread.currentThread, 返回当前线程对象 Thread

### 14.3 线程状态
#### 线程的状态
* New 新创建
* Runnable 可运行
* Blocked 被阻塞
* Waiting 等待
* Timed waiting 计时等待
* Terminated 被终止

可以使用 getState 方法来获得当前线程的状态
```java
package interruptLearn;

public class InterruptTest {
	public static void main(String[] args) {
		TestThread testThread = new TestThread();
		testThread.start();

		for(int i = 0; i < 10; i++) {
			try {
				Thread.sleep(40);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
			System.out.println("testThread 的状态为:" + testThread.getState());
		}
		testThread.interrupt();
		try {
			testThread.join(); // 将主线程阻塞在此，等待子线程
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println("testThread 的状态为:" + testThread.getState());
	}

	private static class TestThread extends Thread {
		@Override
		public void run() {
			int num = 0;
			while (true) {
				if (isInterrupted()) { // 外部中断当前线程后，本线程检测到中断
					System.out.println("当前线程 isInterrupted");
					// 这里虽然被中断了，但是得到的还是 RUNABLE
					System.out.println("testThread 的状态为:" + this.getState());
					// 对外部的中断做出响应，跳出本循环
					break;
				}
				num++;
				try {
					sleep(100);
				} catch (InterruptedException e) {
					 Thread.currentThread().interrupt();
				}
				if (num % 10 == 0) {
					System.out.println("num : " + num);
				}
			}
		}
	}
}
```
以上代码的输出为
```java
testThread 的状态为:TIMED_WAITING
testThread 的状态为:TIMED_WAITING
testThread 的状态为:TIMED_WAITING
testThread 的状态为:TIMED_WAITING
testThread 的状态为:TIMED_WAITING
testThread 的状态为:TIMED_WAITING
testThread 的状态为:TIMED_WAITING
testThread 的状态为:TIMED_WAITING
testThread 的状态为:TIMED_WAITING
testThread 的状态为:TIMED_WAITING
当前线程 isInterrupted
testThread 的状态为:RUNNABLE
testThread 的状态为:TERMINATED
```

#### 线程新建
线程新建即用 `new` 关键字创建线程之后，但是还没有开始运行线程中的代码。还需要准备一些必要的信息，比如方法栈，程序计数器等。

#### 线程可运行
一旦调用了 start 方法，线程开始进入 Runnable 状态，实际上 Runnable 代表两种状态，一种是可运行一种是正在运行。这取决于操作系统的给定的时间。尤其是在抢占式操作系统中。

#### 线程阻塞
线程阻塞是因为遇到 IO 阻塞或者是遇到锁而导致时间片被分割
#### 线程等待
线程等待是主动将时间片让出去，等待被唤醒或者是等待计时结束。
#### 线程终止
- run 运行完，退出线程
- 抛出异常，意外中断本线程

### 14.4 线程属性

####  线程优先级
* 使用 setPriority 方法提高或者降低线程的优先级。从 MIN_PRIORITY 到 MAX_PRIORITY 共 1 到 10 级。常规 NORM_PRIORITY 5.
* 策略：线程调度器每次只选具有较高优先级的线程，即使低优先级的线程从来没有运行过，会造成饥饿现象，应该尽力避免（如何避免呢？？）
```java
static void yield() // 当前线程让步，如果其他可运行的线程具有与此线程相同优先级的，会被调度。
```

#### 守护线程
* 作用：主要为其他线程提供服务。例如计时线程。
* 注意：当只剩下守护线程时，虚拟机就退出了，所以不要在守护线程中访问固有资源，比如文件，数据库等，因为其会在任何时候发生中断。
```java
标识该线程为守护线程或者用户线程，在线程启动之前调用。
```

#### 未捕获异常处理器
线程的 run 方法不能抛出任何被检测的异常（因为无论是从 Thread 或者是 Runnable 中继承而来的 run 方法都没有抛出异常，父方法没有抛出异常，子方法就不能），不被检测的异常会导致线程终止。实际上，在线程死亡之前，已经将异常传递到了用于未捕获异常的处理器。在父线程中是无法直接捕获异常的，如果想捕获异常需要自定义异常捕获器，然后处理异常。为了证明在父线程中无法捕获异常，我做了一个小实验。如下
```java
public class CaughtExceptionTest {

	public static void main(String[] args) {
		try {
			Thread t = new Thread(new InnerThread());
			t.start();
		}catch(RuntimeException e) {
			System.out.println("捕捉到异常了");
		}
	}
}

class InnerThread implements Runnable{
	@Override
	public void run() {
		System.out.println("inner 线程开始运行");
		throw new RuntimeException();
	}
}
```
输出结果为
```java
Exception in thread "Thread-0" inner 线程开始运行
java.lang.RuntimeException
	at priorityLearn.InnerThread.run(PriorityTest.java:43)
	at java.lang.Thread.run(Thread.java:748)
```
可以看出并没有执行父线程中的 catch 语句中的内容。

#####  自定义异常捕获器
[处理器实现一个 Thread.UncaughtExceptionHandler 接口的类。实现其唯一方法](https://www.cnblogs.com/brolanda/p/4725138.html)，该示例中使用了 Excutor 框架，不够直观，下面是我简化之后的例子。
```java
public class CaughtExceptionTest {

	public static void main(String[] args) {
        Thread t = new Thread(new InnerThread());
        t.setUncaughtExceptionHandler(new MyUncaughtExceptionHandler());  // 安装自定义的异常捕获器
        t.start();
	}
}

/**
 * 自定义一个异常捕获器，实现了 Thread.UncaughtExceptionHandler 接口
 * @author bupt632
 *
 */
class MyUncaughtExceptionHandler implements Thread.UncaughtExceptionHandler{
    @Override
    public void uncaughtException(Thread t, Throwable e) {
        System.out.println("caught    "+e);
    }
}

class InnerThread implements Runnable{
	@Override
	public void run() {
		System.out.println("inner 线程开始运行");
		throw new RuntimeException();
	}
}
```
运行结果为:
```java
inner 线程开始运行
caught    java.lang.RuntimeException
```

除了自定义异常处理器之外还可以选择
* 默认处理器，可以使用
```java
    setDefaultUncaughtExceptionHandler 为所有线程安装一个默认的处理器，替换处理器可使用日志 api 发送未捕捉异常的报告到日志中。
```
* 不安装默认处理器
    默认的处理器为空，不为独立的线程安装处理器，此时的处理器就是该线程的 ThreadGroup 对象。但是不推荐使用

### 14.5 同步
两个线程并发地修改同一个对象的状态，这就引起了竞争条件，引起了竞争就会使得对象的状态出现问题，最常见的一个例子就是多个线程并发对一个变量加 1。如下
```java
package concurrencyLearn;

public class ConcurrencyErrorSituation {

	public static void main(String[] args) throws InterruptedException {
		ModifiedObject mo = new ModifiedObject();

		TestThread t1 = new TestThread(mo);
		TestThread t2 = new TestThread(mo);
		TestThread t3 = new TestThread(mo);
		t1.start();
		t2.start();
		t3.start();

		t1.join();
		t2.join();
		t3.join();

		System.out.println("i 最终的值为：" + mo.get());
	}
}

class ModifiedObject{
	private int i = 0;
	void increaseOne() {
		i++;
	}
	int get() {
		return i;
	}
}

class TestThread extends Thread{
	ModifiedObject mo;
	public TestThread(ModifiedObject mo) {
		this.mo = mo;
	}
	@Override
	public void run() {
		for(int i = 0; i < 10000; i++) {
			mo.increaseOne();
		}
	}
}
```
运行结果为:
```java
i 最终的值为：14934
```
三个线程各使 i 自增 10000 次，我们期望的值应该是 30000，但是实际上输出的值却是 14934，小于 30000。引起该现象的原因需要从 `ModefiedObject` 中的 `increaseOne` 函数中的 `i++` 开始说起，实际上 `i++` 并不是一个原子操作，而是三个操作
1. 从主内存中取出 i 的值到工作内存（每个线程有各自对应的工作内存）
2. 计算出 i + 1 的值，现在是在线程对应的工作内存中
3. 将 i + 1 写回主内存中

在同一时刻中，很可能 a 线程处于 3 阶段，而 b 线程处于 2 阶段，二者此时的 i 值是相同的，当 a 线程执行完 3 时，将 i + 1 写入到主内存中后， i 值变成了 i + 1，而 b 在 2 阶段中的 i 还是原来的 i，并非主内存中的 i，因此当 b 执行完 i + 1 操作之后，再执行 3 阶段相当于还是将 i + 1 写入内存而非我们所期望的 i + 1 + 1，虽然外观上是 a b 线程对 i 分别加了两次，实际上是 b 线程的操作覆盖了 a 线程的操作。理想情况下，我们希望将 i++ 这个操作变成原子性的操作。


* 资源，竞争，出错
	为了将一系列操作封装成原子操作，使其不可被打断，java 提供了两种方式，其一为 synchronized关键字，synchronized关键字自动提供一个锁和相关条件，其二为 ReentrantLock 保护代码块。

* 锁对象
    ```java
    Lock myLock = new ReentrantLock(); // 如果使用 ReentrantLock(boolean fair)，将生成一个公平锁，但是此锁的性能很低。
    myLock.lock();
    try{
    some operations;
    }
    finally
    {
     myLock.unlock(); // 这一结构确保任何时刻都只有一个线程进入临界区，其他线程会被阻塞。因为这个特点，unlock一定要放在 finally，否则其他线程将永远被阻塞
     // 每个调用 try 中语句的代码将串行执行
    }
    ```
    如果使用锁，就不能使用带资源的 try 语句？

* 条件对象
	* 出现的原因：
		当线程借助锁，进入了临界区，却发现在某一条件满足之后，它才能执行。但是此时它已经将资源锁定，其他线程被阻塞，无法得到这些资源，因而无法运行。故需要使用一个条件来管理那些已经获得了一个锁但是不能做有用工作的线程，让其将资源让给其他线程。
    * 实现方式：
    ```java
        private Condition sufficientFunds;
        sufficientFunds = bankLock.newCondition();
        // 当发现条件不满足时，调用
        sufficientFunds.await();
        // 当其他线程操作之后，上面条件可能被满足的时候，调用下面方法，让所有那些阻塞的线程重新测试条件
        sufficientFunds.signalAll();
        // 如果没有线程调用上面函数，那么所有正在等待的线程会一直被阻塞，出现饥饿现象，最终被挂起。
        // 当然还可以调用 signal 函数，随机选择一个正在等待线程测试条件，该函数具有一定的风险，即如果随机选的那个不能解除阻塞呢？
        sufficientFunds.singal();
	```
* 锁和条件的总结：
	* 锁可以用来保护代码片段，任何时刻只有一个线程执行被保护代码
	* 锁可以管理视图进入被保护代码段的线程
	* 锁可以拥有一个或多个相关的条件对象
	* 每个条件对象管理那些已经进入被保护的代码段但还不能运行的线程

* synchronized 关键字
	* 实现基础：
		java 语言内部的机制，从 1.0 开始，java 中的每个对象都有一个内部锁，如果对一个方法用 synchronized 关键字声明，那么该锁将保护整个方法中的代码
    * 特点：
    	内部锁只有一个相关条件，使用 wait 方法将一个线程添加到等待集中，使用 notifyAll 或者 notify 解除线程阻塞状态。

* 内部锁和条件的局限
	* 不能中断一个正在尝试获得所的线程
	* 试图获得锁时不能设置超时限制的时间
	* 每个锁仅有单一的条件可能是不够的
* 建议：
	如果能够使用 synchronized 关键字，就尽量使用该关键字，如此能够减少代码量和出错数。

* 同步阻塞
	线程可以通过调动同步方法获得锁，例如
    ```java
    private Object lock = new Obejcet();
    ...
    public void transfer(int from, int to, int amount){
    	...
    	synchronized(lock){
        	account[from] -= amount;
            account[to] += amount;
        }
        ...
    }
    ```
    * 分析：
    	此处 lock 仅仅是用来使用每个 java 对象中持有的内部锁。使用对象的锁（该锁可以是内部锁）来实现原子操作，被称为客户端锁定。使用的前提是该对象中的所有原子操作也均是由内部锁来实现的，否则会出现问题，这是客户端锁定的脆弱之处。所以需要其他的机制来替代。

* 监视器
	锁和条件是面向函数，而监视器面向对象，监视器的最大的特点是该锁对对象所有的方法进行加锁。

* Volatile 域
	* 原因：
		1. 多处理器的计算机能够暂时在寄存器或者本地内存缓冲器中保存内存中的值，多处理器的计算机处理多线程程序时，运行在不停的处理器上的线程可能在同一时刻从相同的内存地址中取到不同的值，因为该内存地址的值可以从不同的地方（例如寄存器或者是本地的内存缓冲器，而不是真正的内存中）取得。
		2. 编译器可以改变指令执行顺序来使得吞吐量最大化。而指令重排的前提是单线程，只有当前线程可以修改内存中的值，但是现在的程序是面向多线程，会可能有多个线程来修改这个值。
	1. [保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。](https://www.cnblogs.com/dolphin0520/p/3920373.html)，保证了在 t 时刻，所有线程读取该变量的值均相同。
	2. 禁止进行指令重排序。
	3. 不提供原子性
	```java
    private volatile boolean done;
    ```
    > 如果对共享变量除了赋值之外不做其他操作，可以将这些变量声明为 volatile

* final 变量
	从多线程中安全地访问一个共享域，即这个域声明为 final 时。（将一个向量数组声明为final后，还是可以修改其中的内容，但是不可以重新让该变量名指向另一个向量数组）

* 原子性
	很多类提供给了原子方法，比如 java.util.concurrent.atomic.AtomicInteger类，提供了整数自增自减等操作，相似的还有 AtomicBoolean等

* 死锁

* 线程局部变量

	* 原因：
		因为并非所有的对象都是线程安全的，并发访问可能破坏对象中的数据结构
	* 使用 ThreadLocal 辅助类为各个线程提供各自的实例
	```java
    // 可以使用以下代码
    public static final ThreadLocal<SimpleDateFormat> dataFormat =
    new ThreadLocal<SimpleDateFormat>()
    {
    	protected SimpleDateFormat initialValue(){
        	return new SimpleDateFormat("yyyy-MM-dd");
        }
    };
    ...
    // 访问时使用
    String dateStamp = dateFormat.get().format(new Date());
    // 第一次调用时，会调用 initialValue 方法，此后 get方法 会返回属于当前线程的实例。
    ```

* 锁测试和超时
	* tryLock 方法，试图申请一个锁，否则返回 false，然后线程可以去做其他的事情。
	* 获得锁的过程中被中断
		* Lock 方法不能被中断，当线程在尝试获得锁的时候被中断，中断线程在获得锁之前会一直处于阻塞状态。如果出现死锁，那么lock就无法终止。
		* tryLock，带有超时参数的话，超时之后，其将在等待期间被中断，会抛出 InterruptedException 异常。
		* lockInterruptibly 方法，相当于一个超时设为无限的 tryLock 方法。
		* myCondition.await(100,TimeUnit.MILLISECONDS),等待一个条件的时候也可以设定超时时间。

* 读写锁
	* 适用场景：如果很多线程从给一个数据结构中读取数据而很少线程修改其中的数据
	* 适用方式：
	```java
    	private ReentrantReadWriteLock rwl = new ReentrantReadWriteLock();
        private Lock readLock = rwl.readLock();
        private Lock writeLock = rwl.writeLock();
    ```

* 为何弃用 stop 方法和 suspend 方法
	* stop 方法很不安全，调用 stop 方法时，该方法终止所有未结束的方法。当线程被终止，立即释放被它锁住的所有对象的锁，这会导致对象处于不一致的状态。
	* suspend 挂起一个持有一个锁的线程时，那么该锁在恢复之前是不可用的，其他等待该锁的线程处于阻塞状态，程序死锁。

##### 14.6 阻塞队列

* 出现的原因：
	作为一个高层的开发者，应该尽可能地远离底层构建块，应该尽可能使用并发处理的专业人士实现的较高层次的结构，这样更安全，更方便。
* 特点：
	生产者线程向队列插入元素，消费者线程从队列中取出，使用队列可以安全地从一个线程向另一个线程传递数据。该过程中不需要线程同步，因为队列已经保证了同步。

