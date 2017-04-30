---
layout: default
title: 工厂模式
---

![](http://oou15cuq6.bkt.clouddn.com//image/pattern/%E6%9C%BA%E6%A2%B0%E9%BD%BF%E8%BD%AE.jpg)

> 定义:定义一个用于创建对象的接口，让子类决定实例化哪一个类。工厂方法使一个类的实例化延迟到其子类

不同子类可能会有不同的实现，不同的代码，在不修改已存在类情况下，去扩展已有类的方法，并且实例化的过程延迟到客户端代码执行过程中，即多态。。

才规矩，上个例子，依然来自《大话设计模式》

首先是一个类，可以认为是已存在的不可修改的类，可能来自网络/文本/jar包等等
```
public class LeiFeng
{
	public void sweep()
	{
		System.out.println("扫地");
	}

	public void wash()
	{
		System.out.println("洗衣");
	}
}
```

一种实现，这里是学生学 LeiFeng 做好事
```
public class Undergraduate extends LeiFeng
{

	@Override
	public void sweep()
	{
		System.out.println("undergraduate sweep");
	}

	@Override
	public void wash()
	{
		System.out.println("undergraduate wash");
	}

}

```


另一种实现，这里是志愿者学 LeiFeng 做好事
```
public class Volunteer extends LeiFeng
{
	@Override
	public void sweep()
	{
		System.out.println("volunteer sweep");
	}

	@Override
	public void wash()
	{
		System.out.println("volunteer wash");
	}
}

```


工厂方法，创建一个 LeiFeng
```
public interface IFactory
{
	LeiFeng createLeiFeng();
}
```

创建哪个 LeiFeng，这里创建学生
```
public class UndergraduateFactory implements IFactory
{

	public LeiFeng createLeiFeng()
	{
		return new Undergraduate();
	}

}
```


创建哪个 LeiFeng ，这里创建志愿者
```
public class VolunteerFactory implements IFactory
{

	public LeiFeng createLeiFeng()
	{
		return new Volunteer();
	}

}
```

客户端代码，可以修改factory, 的实现，如`factory = new UndergraduateFacotry()`， 就可以创建一个学生 LeiFeng ，这样不需要修改已有代码，如果来一个男 LeiFeng，只需要加 `Male`继承LeiFeng，`MaleFactory` 实现 IFactory 创建方法即可
```
public class FactoryMain
{
	public static void main(String[] args)
	{
		IFactory factory = new VolunteerFactory();
		LeiFeng student = factory.createLeiFeng();

		student.wash();
		student.sweep();
	}
}

```

## 总结
将创建具体对象延迟到客户端调用时，这样工厂类中就不需要加业务代码。  

