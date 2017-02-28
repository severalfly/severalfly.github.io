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


