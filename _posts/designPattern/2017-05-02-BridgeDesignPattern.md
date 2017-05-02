---
layout: default
title: 桥接模式
---


定义：将抽象与它的实现分离，使它们都可以独立地变化。

具体类通过基类中组合的其他功能的实现来调用不同实现的具体实现，  
一个例子：不同品牌的手机使用手机这个基类提供的软件，这些软件只是虚基类，会有不同的实现功能（MP3/通讯录/游戏等等）

代码来说话


// 目标不同实现的基类，功能都来自于此，此为软件的基类，这里的软件通用点
```java
public abstract class HandsetSoft
{
	public abstract void running();
}
```


游戏软件
```java
public class HandSetGame extends HandsetSoft
{
	@Override
	public void running()
	{
		System.out.println("开始运行手机游戏");
	}
}
```


通讯录
```java
public class HandSetAddressList extends HandsetSoft
{
	@Override
	public void running()
	{
		System.out.println("手机通讯录");
	}
}
```


设备的基类，其他手机品牌会继承此类，做对应扩展
```java
public abstract class HandSetBrand
{
	protected HandsetSoft soft;

	public HandSetBrand(HandsetSoft soft)
	{
		super();
		this.soft = soft;
	}
	public abstract void running();
	// setters && getters
}
```


手机品牌M
```java
public class HandSetBrandM extends HandSetBrand
{
	public HandSetBrandM(HandsetSoft soft)
	{
		super(soft);
	}
	@Override
	public void running()
	{
		this.soft.running();
	}
}
```



手机品牌N
```java
public class HandSetBrandN extends HandSetBrand
{
	public HandSetBrandN(HandsetSoft soft)
	{
		super(soft);
	}
	@Override
	public void running()
	{
		this.soft.running();
	}
}
```


客户端也就简单了，brand 可以实例化M也可以实例化N，软件可以使用游戏也可以使用通讯录
```java
public class BridgeMain
{
	public static void main(String[] args)
	{
		HandSetBrand brand = new HandSetBrandM(new HandSetGame());
		brand.running();

		brand.setSoft(new HandSetAddressList());
		brand.running();
	}
}
```


在系统设计中，能使用组合的，不使用继承；继承具有强关系，父类任何修改都会影响到子类，而组合这种模式，灵活多了，并且实现代码也比较简单

之前实习老板跟我们交流，就让多使用组合，今天看到桥接模式，终于理解当时老板的想法了。不过现在（2017-05-02 22:20:28）还没有很理解这种实现方式，

