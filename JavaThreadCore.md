---
layout: default
---

[TOC]

# 第二章
## synchronized 
### 方法内的变量为线程安全

在两个 线程访问同一个对象中的同步方法时，一定是线程安全的；  
这个前提是同步方法  

多个线程访问多个对象，则JVM 创建多个锁。

### synchronized 方法与锁对象
>如果A线程先持有object 对象的Lock 锁，B线程可以以异步的方式调用 object 对象中的非synchronized 类型的方法  
>如果A线程先持有object 对象的Lock 锁，B 线程如果在这时调用 object 对象中的synchronized 类型的方法则需等待，也就是同步；

同步使用synchronized 方法就没问题了，这貌似是个大方法，消耗资源过多，且看后面文章分析

### 脏读
> 当A 线程调用anyObject 对象加入synchronized 关键字的X方法时，A线程就获得了X 方法锁，更准备地讲，是获得了对象的锁，所以其他线程必须等待A 线程执行完毕，才可以调用X 方法， 但B 线程可以随意调用其他的非synchronized 同步方法；  

> 当A 线程调用anyObject 对象加入 synchronized 关键字的X 方法时，A 线程就获得了X 方法所在对象的锁，所以其他线程必须等A 线程执行完毕才可以调用X 方法，而B 线程如果调用声明了 synchronized 关键字的非X 方法时，必须等A 线程X 方法执行完，也就是释放对象锁后，才可以调用。这时A 线程已经执行了一个完整的任务，也就是说username 和 password 这两个实例变量已经同时被赋值，不存在脏读的基本环境

### synchronized 锁重入
> 当一个线程得到一个对象锁后，再次请求此对象锁时是可以再次得到该对象的锁的。这也证明在一个 synchronized  方法/块的内部调用本类的其他 synchronized 方法/块时，是永远可以得到锁的

### 出现异常，锁自动释放
> 当一个线程执行的代码出现异常时，其所持有的锁会自动释放

### 同步不具有继承性
这个不理解

### synchronized 某些弊端
synchronized 声明方法在某些情况下是有弊端的，比如A线程调用同步方法执行一个长时间的任务，那么B线程则必须等待比较长时间。在这样的情况下可以使用*synchronized同步语句块*

### 虽然线程A 先持有了object 对象的锁，但线程B完全可以异步调用非 synchronized 类型的方法
>如果A 线程先持有object 对象的Lock 锁，B 线程如果在这时调用object 对象中的 synchronized 类型的方法，则需等待，也就是同步了

### synchronized 代码块间的同步性
> 在使用同步 synchronized(this) 代码块时需要注意的是，当一个线程访问object 的一个 synchronized(this) 同步代码块时，其他线程对同一个 object 中所有其他 synchronized(this) 同步代码块的访问将被阻塞，这说明 synchronized 使用的 “对象监视器”是一个。

### synchronized(this) 代码块是锁定当前对象的

### 将任意对象作为对象监视器
多个线程调用同一个对象中的不同名称的 synchronized同步方法或者 synchronized(this) 同步代码块时，调用的效果就是按顺序执行，也就是同步的阻塞的。

synchronized 同步方法或 synchronized(this) 同步代码块分别有两种作用

1. synchronized 同步方法
	* 对其他 synchronized 同步方法或 synchronized(this) 同步代码块调用呈阻塞状态。
	* 同一时间只有一个线程可以执行 synchronized 同步方法中的代码
2. synchronized(this) 同步代码块
	* 对其他 synchronized(this) 同步方法或 synchronized(this) 同步代码块调用呈阻塞状态
	* 同一时间只有一个线程可以执行 synchronized(this) 同步代码块中的代码

### 多个线程调用同一个方法是随机的

### synchronized(非this对象x)  
这种格式的写法是将x 对象本身作为“对象监视器”，这样就可以得出以下3个结论：

1. 当多个线程同时执行 `synchronized(x){}`同步代码块时呈同步效果；
2. 当其他线程执行x对象中 synchronized 同步方法时呈同步效果
3. 当其他线程执行 x 对象方法里面的 synchronized(this)  代码块时，也呈现同步效果


### 静态同步 synchronized 方法与 synchronized(class) 代码块
synchronized 关键字保到 static 静态方法上是给Class 类上锁，而 synchronized 关键字加到非static 静态方法上是给对象上锁  
目测这两个有本质的不同


### String 常量池带来的问题

两个线程拥有相同锁，所以会造成B 不能执行，即两个线程都锁相同的String 时，就会存在这机关报问题
[有代码为证](https://github.com/severalfly/MyTest/tree/master/JavaLearning/JAVA%E5%A4%9A%E7%BA%BF%E7%A8%8B%E7%BC%96%E7%A8%8B%E6%A0%B8%E5%BF%83%E6%8A%80%E6%9C%AF-JAVA%20Core/src/main/java/org/ch2/stringAndSyn)


### volatile 
会保证数据的可见性，但是不保证数据的原子性，即，一个线程修改可以使用其他所有线程同步修改；但是，多个线程同时修改时，就会造成脏读。  
这个关键字适用于一个主线程控制其他线程的运行与停止的情况，其他的情况慎重使用，  
目前的 synchronized 性能也得到大的提升，

volatile 不保证原子性，像 i++ 这样的，i = i + 1 就不适用

![test](https://cl.ly/1K320q2c0b24/Image%202017-03-14%20at%207.58.23%20PM.png)
> 在多线程环境中， use 和 assign 是多次出现的，但这一操作并不是原子性，也就是在read 和 load 之后，如果主内存count 变量发生修改之后，线程工作内存中的值由于已经加载，不会产生对应的变化，也就是私有内存和公共内存中的变量不同步，所以计算出来的结果会和预期不一样，也就出现了非线程安全问题

![testImage](https://cl.ly/1b1p0S3Q1f3Q)

# 第三章

### notify 并不会立即释放锁
notify 之后会继续持有锁，直到锁范围结束


###
生产者消费者模型直接使用notify 会造成假死现象，注意这种在使用时要用`notifyAll`


### 通过管道进行线程间通信：字节流
线程之间可以通过管道进行通信，关键一步，`out.connect(input);`使输入与输出相联接。[示例代码](https://github.com/severalfly/MyTest/tree/master/JavaLearning/JAVA%E5%A4%9A%E7%BA%BF%E7%A8%8B%E7%BC%96%E7%A8%8B%E6%A0%B8%E5%BF%83%E6%8A%80%E6%9C%AF-JAVA%20Core/src/main/java/org/ch3/pipeInputOutput)


### join 方法 
Thread.join() 方法是可以等待前一个线程执行完毕后，才开始执行的；
Thread.join(time) 最长等待 time 毫秒，注意这个东西与`interrupt()`可能会产生异常


### sleep 方法不会释放锁，join 方法会释放对象锁

## ThreadLocal 的使用
`ThreadLocal ` 解决的是变量在不同线程间的隔离性，也就是不同线程拥有自己的值，不同线程中的值是可以放入 `ThreadLocal` 中进行保存的。  
这个变量是可以将线程间隔离，一个线程可以保存自己的变量，这个刚好在项目中使用到了，  
在给用户发push时，一般情况下一个任务就可以满足条件，但是在某些时候，是需要给大量用户发push，这就造成一个问题，可能有多个push 任务同时到达，这样后到达的，就会失败。这样可以使用一个线程变量，使之等待；大量push发完后，将此线程变量清空，又回到无阻塞发送，减轻系统压力



## 关于线程安全与非线程安全的思考
1. 线程安全，是指一个方法内的安全，如线程安全类自增长，保证方法内的安全，
2. 非线程安全，是指一个方法内可能出现意外，如 `simpleDateFormat` 的 format 方法，方法中会出现多线程竞争的情况，也就出现了异常 
