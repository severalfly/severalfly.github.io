---
layout: default
title: 外观模式
---


![](http://oou15cuq6.bkt.clouddn.com//image/pattern/20160104165258819.png)

定义：为子系统的一组接口提供一个一致界面。此模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。


还是上个例子


首先定义三个子系统，每个子系统提供一个方法，实际生产中可能有多个不同方法
```java
public class SubSystem1
{
	public void method1()
	{
		System.out.println("method1");
	}
}
```

```java
public class SubSystem2
{
	public void method2()
	{
		System.out.println("method2");
	}
}
```

```java
public class SubSystem3
{
	public void method3()
	{
		System.out.println("method3");
	}
}
```


外观类，提供了两个方法，分别对应两组不同的运算，使得客户端调用时可以屏蔽对子系统的直接调用
```java
public class Facade
{
	private SubSystem1 sub1;
	private SubSystem2 sub2;
	private SubSystem3 sub3;

	public Facade()
	{
		super();
		this.sub1 = new SubSystem1();
		this.sub2 = new SubSystem2();
		this.sub3 = new SubSystem3();
	}

	public void methodA()
	{
		System.out.println("第一组");
		sub1.method1();
		sub2.method2();
		sub3.method3();
	}

	public void methodB()
	{
		System.out.println("第二组");
		sub2.method2();
		sub3.method3();
	}
}

```

客户端调用时，只要调用外观类就可以鸟
```java
public class FacadeMain
{
	public static void main(String[] args)
	{
		Facade facade = new Facade();
		facade.methodA();
		facade.methodB();
	}
}
```

总结：这个模式主要是用在客户端对子系统不好直接访问，或者访问权限受限时，
