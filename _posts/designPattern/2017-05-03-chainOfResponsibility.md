---
layout: default
title: 职责连模式
---


![](http://oou15cuq6.bkt.clouddn.com/image/pattern/%E8%81%8C%E8%B4%A3%E9%93%BE%E6%A8%A1%E5%BC%8F.png)

定义：使多个对象都机会处理请求，从而避免请求的发送者和接收者之间的耦合关系。将这个对象连成一个链，并沿着这条链传递请求，直到有一个对象处理它为止。

从定义看，与状态模式很相近，这个也需要抽时间找下区别


来个例子


首先定义处理基类
```java

public abstract class Handler
{
	protected Handler successor;
	// setter && getter
	public abstract void handleRequest(int request);

}
```

具体类1，处理0~10
```java
public class ConcreteHandler1 extends Handler
{
	@Override
	public void handleRequest(int request)
	{
		if (request > 0 && request < 10)
		{
			System.out.println("handler1处理请求" + request);
		}
		else if (this.successor != null)
		{
			this.successor.handleRequest(request);
		}
	}
}
```


具体类2，处理10~20
```java
public class ConcreteHandler2 extends Handler
{
	@Override
	public void handleRequest(int request)
	{
		if (request >= 10 && request < 20)
		{
			System.out.println("handler2处理请求" + request);
		}
		else if (this.successor != null)
		{
			this.successor.handleRequest(request);
		}
	}
}
```


具体类3，处理20~
```java
public class ConcreteHandler3 extends Handler
{
	@Override
	public void handleRequest(int request)
	{
		if(request >=20)
		{
			System.out.println("handler3开始处理了" + request);
		}
	}
}
```


客户端调用时，需要指定当前类的上级，即处理不了时，调用何方法处理
```java
public class ChainMain
{
	public static void main(String[] args)
	{
		Handler h1 = new ConcreteHandler1();
		Handler h2 = new ConcreteHandler2();
		Handler h3 = new ConcreteHandler3();
		h1.setSuccessor(h2);
		h2.setSuccessor(h3);

		List<Integer> list = new ArrayList<Integer>();
		list.add(2);
		list.add(5);
		list.add(14);
		list.add(22);
		list.add(18);
		list.add(3);
		list.add(27);
		list.add(20);
		for (Integer i : list)
		{
			h1.handleRequest(i);
		}

	}
}

```


与状态模式很相近，貌似还没理解这两种模式的区别，回头一定要找下这个区别
