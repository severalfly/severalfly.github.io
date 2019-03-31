---
layout: default
title: 设计模式的艺术--单例模式
---

> 单例模式：确保某一类只有一个实例，而且自行实例化并向整个系统提供这个实例，这个类称为单例类，它提供全局访问的方法。是一种对象创建模式。

主要有三个特点：  

1. 某个类只有一个实例。
2. 它必须自行创建这个实例
3. 它必须自行向整个系统提供这个实例

有三种方法可以创建单例：

1. 饿汉模式，即在类被加载时创建
2. 懒汉模式，即在第一次使用时创建
3. IoDH ，在单例类内部创建一个静态内部类，其唯一作用就是持有单例类的引用


典型代码：前两个方式，网上的代码太多，这里只贴出第三种方式的代码

```
public class SingletonV2
{
	private SingletonV2()
	{
		// 定义私有的构造方法
	}

	private static class HolderClass
	{
		private final static SingletonV2 instance = new SingletonV2();
	}

	public static SingletonV2 getInstance()
	{
		return HolderClass.instance;
	}
}
```

测试代码
```
public static void main(String[] args)
{
	SingletonV2 s1, s2;
	s1 = SingletonV2.getInstance();
	s2 = SingletonV2.getInstance();
	System.out.println(s1 == s2);
}
```


### 应用场景
1. 系统只需要一个实例，如提供唯一序列号生成器或资源管理器；
2. 客户调用类的单个实例只允许使用一个公共访问点，除了公共对象访问点，不能通过其他途径访问该实例。