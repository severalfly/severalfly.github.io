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






一起学习JAVA
<a target="_blank" href="//shang.qq.com/wpa/qunwpa?idkey=11c2e67fa3a7a504fff4a17c3fb89185d5a1fcf23ac13570a371551d24ef04dd"><img border="0" src="//pub.idqqimg.com/wpa/images/group.png" alt="一起学JAVA吧" title="一起学JAVA吧"></a>
