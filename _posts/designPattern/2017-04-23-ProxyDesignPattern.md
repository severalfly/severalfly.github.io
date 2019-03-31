---
layout: default
title: 代理模式
---

> 定义：为其他对象提供一种代理以控制对这个对象的访问

先直接上个例子，来源 程杰《大话设计模式》

# 首先定义代理类与实体类的接口  
代码中需要代码与实体都实现此接口，只是代理类直接调用实体类，由实体类处理
```
public interface GiveGift
{
	void giveDolls();

	void giveFlowers();

	void giveChocolate();
}
```


`SchoolGirl`这只是个此例子中的一个说明，Pursuit 需要向此类做操作
```
public class SchoolGirl
{
	private String name;

	public SchoolGirl(String name)
	{
		super();
		this.name = name;
	}

	public String getName()
	{
		return name;
	}

	public void setName(String name)
	{
		this.name = name;
	}

}
```


被代理的对象，需要实现giveGift 接口，负责实际的操作
```
public class Pursuit implements GiveGift
{
	private SchoolGirl mm;

	public Pursuit(SchoolGirl mm)
	{
		super();
		this.mm = mm;
	}

	public void giveDolls()
	{
		System.out.println(this.mm.getName() + " give u a doll");
	}

	public void giveFlowers()
	{
		System.out.println(this.mm.getName() + " give u a flower");
	}

	public void giveChocolate()
	{
		System.out.println(this.mm.getName() + " give u a chocolate");
	}
}

```


代理类，实现GiveGift接口，拥有被代理类的引用，接口实现方法完全调用被代理类即可，这也是代理模式的一个简单之处
```
public class Proxy implements GiveGift
{
	private Pursuit gg;

	public Proxy(SchoolGirl mm)
	{
		super();
		this.gg = new Pursuit(mm);
	}

	public void giveDolls()
	{
		gg.giveDolls();
	}

	public void giveFlowers()
	{
		this.gg.giveFlowers();
	}

	public void giveChocolate()
	{
		this.gg.giveChocolate();
	}

}
```


客户端代码
```
public class ProxyMain
{
	public static void main(String[] args)
	{
		SchoolGirl mm = new SchoolGirl("qiaoqiao");
		mm.setName("qiaoqiao");

		Proxy proxy = new Proxy(mm);
		proxy.giveDolls();
		proxy.giveFlowers();
		proxy.giveChocolate();
	}
}
```

总结
=
代理模式会用在以下几种场合  

1. 远程代理，也就是为一个对象在不同的地址空间提供局部代表，这样可以隐藏一个存在于不同地址空间的事实	
2. 虚拟代理，是根据需要创建开销很大的对象，通过它来存放实例化需要很长时间的真实对象
3. 安全代理，用来控制真实对象访问时的权限
4. 智能指引，指当调用真实的对象时，代理处理另一些事

这种模式是在客户端不好直接访问被代理对象时使用，被代理对象可能会执行时间过长/不安全/有权限限制，这样使用代理就可以避免客户端的延迟现象
![](http://oou15cuq6.bkt.clouddn.com//image/pattern/%E4%BB%A3%E7%A0%81%E6%A8%A1%E5%BC%8F%E5%B0%8F%E5%85%AD%E8%A1%A8%E7%99%BD_meitu_1.png)
