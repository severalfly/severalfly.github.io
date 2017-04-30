---
layout: default
title: 适配器模式
---


定义：将一个类的接口转换成客户希望的另外一个接口。`Adapter`模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。

使用场景：需要的东西就在眼前，但却不能使用，而短时间又无法改造它，于是我们就想办法适配它。  
系统的数据和行为都正确，但接口不符时，我们应该考虑用适配器，目的是使控制范围之外的一个原有对象与某个接口匹配。适配器模式主要应用于希望利用一些现存的类，但是接口又与利用环境要求不一致的情况，比如在早期代码利用些功能等应用上很有实际价值


还是来个例子说明问题


虚基类，所有方法的基础
```java
public abstract class Player
{
	protected String name;

	public Player(String name)
	{
		super();
		this.name = name;
	}

	public abstract void attack();

	public abstract void defense();
}
```


定义篮球的前锋
```java
public class Forwards extends Player
{

	public Forwards(String name)
	{
		super(name);
	}

	@Override
	public void attack()
	{
		System.out.println("前锋" + this.name + "进攻");
	}

	@Override
	public void defense()
	{
		System.out.println("前锋" + this.name + "防守");

	}

}
```


外籍中锋，如姚明
```java
public class ForeignCenter
{
	private String name;

	public String getName()
	{
		return name;
	}

	public void setName(String name)
	{
		this.name = name;
	}

	public void 进攻()
	{
		System.out.println("外籍中锋" + this.name + "进攻");
	}

	public void 防守()
	{
		System.out.println("外籍中锋" + this.name + "防守");
	}
}
```


翻译，此类作用类似于适配器
```java
public class Translator extends Player
{
	ForeignCenter fc = new ForeignCenter();
	public Translator(String name)
	{
		super(name);
		fc.setName(name);
	}

	@Override
	public void attack()
	{
		fc.进攻();
	}

	@Override
	public void defense()
	{
		fc.防守();
	}

}
```


教练开始发号施令
```java
public class AdapterMain
{
	public static void main(String[] args)
	{
		Player b = new Forwards("巴蒂尔");
		b.attack();

		Player m = new Translator("姚明");
		m.attack();
		m.defense();
	}
}
```


将相同功能不同接口的类拿来用，这种仅用在源代码修改难度过大的情况，系统设计之初尽可能少用，也不是不可用，只是需要特别注意，用到的说明就可能存在设计的漏洞，应该及时重构修改




