---
layout: default
title: 中介者模式
---


直接来个例子


首先，定义个基类国家的概念，包含声明方法与接收消息方法；并且保存有联合国的引用
```java
public abstract class Country
{
	protected UnitNations unitNations;

	public Country(UnitNations unitNations)
	{
		super();
		this.unitNations = unitNations;
	}

	public abstract void declare(String msg);

	public abstract void getmsg(String msg);
}
```


好事者美国，可以声明消息与接收消息
```java
public class USA extends Country
{

	public USA(UnitNations unitNations)
	{
		super(unitNations);
	}

	@Override
	public void declare(String msg)
	{
		this.unitNations.sendMsg(msg, this);
	}

	@Override
	public void getmsg(String msg)
	{
		System.out.println("USA获得信息" + msg);
	}
}
```


尹拉克就比较惨了，也可以声明消息与接收消息
```java
public class Iraq extends Country
{
	public Iraq(UnitNations unitNations)
	{
		super(unitNations);
	}

	@Override
	public void declare(String msg)
	{
		this.unitNations.sendMsg(msg, this);
	}

	@Override
	public void getmsg(String msg)
	{
		System.out.println("Iraq获得消息" + msg);
	}
}

```


联合国组织，可能会有多个具体组织的
```java
public abstract class UnitNations
{
	public abstract void sendMsg(String msg, Country cty);
}
```

联合国了，主要作用就是判断消息应该由那个国家接收，发到相应的国家，这里做的简单，实际应该把消息目的国家放到消息体中的，但是实现麻烦了
```java
public class UnitNationsSecurityCouncil extends UnitNations
{
	private Country Usa;
	private Country Iraq;
	// setters && getters
	@Override
	public void sendMsg(String msg, Country cty)
	{
		if (cty == this.Usa)
		{
			this.Iraq.getmsg(msg);
		}
		else
		{
			this.Usa.getmsg(msg);;
		}
	}
}
```


客户端一向简单至上
```java
public class MediatorMain
{
	public static void main(String[] args)
	{
		UnitNationsSecurityCouncil unsc = new UnitNationsSecurityCouncil();
		Country c1 = new USA(unsc);
		Country c2 = new Iraq(unsc);

		unsc.setUsa(c1);
		unsc.setIraq(c2);

		c1.declare("开打");
		c2.declare("别打我");
	}
}
```



