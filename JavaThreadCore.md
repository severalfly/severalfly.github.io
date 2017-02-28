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


