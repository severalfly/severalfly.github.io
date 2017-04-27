---
layout: default
title: 建造者模式
---

![](http://oou15cuq6.bkt.clouddn.com//image/pattern/%E5%BB%BA%E9%80%A0%E8%80%85.jpg)

定义：将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示

依然是来个例子


首先是我们要构建的产品，包含一个添加方法与显示方法
```java
public class Product
{
	private List<String> parts = new ArrayList<String>();

	public void add(String part)
	{
		this.parts.add(part);
	}
	public void show()
	{
		System.out.println("\n产品创建---");
		for (String string : this.parts)
		{
			System.out.println(string);
		}
	}
}

```


然后是个建造者类，这是个虚类，所有方法都是虚的，子类必须实现所有方法才行，这就避免了在实际创建时缺少某些步骤的情况
```java
public abstract class Builder
{
	abstract public void bilderPartA();

	abstract public void bilderPartB();

	abstract public Product getResult();
}
```



第一个具体类，实现父类的方法，也就是给产品加些字符串，也是在实际代码中给产品增加的新内容
```java
public class ConcreteBuilder1 extends Builder
{
	private Product product = new Product();

	@Override
	public void bilderPartA()
	{
		this.product.add("部件A");
	}

	@Override
	public void bilderPartB()
	{
		this.product.add("部件B");

	}

	@Override
	public Product getResult()
	{
		return this.product;
	}
}
```


第二个子类，同上
```java
public class ConcreteBuilder2 extends Builder
{
	private Product product = new Product();

	@Override
	public void bilderPartA()
	{
		this.product.add("部件K");
	}

	@Override
	public void bilderPartB()
	{
		this.product.add("部件L");

	}

	@Override
	public Product getResult()
	{
		return this.product;
	}
}
```



指挥官，这牛逼角色，控制着构建过程，
```java
public class Director
{
	public void construct(Builder builder)
	{
		builder.bilderPartA();
		builder.bilderPartB();
	}
}
```


客户端就很简单了，感觉与前面的几种模式有很大的相同点
```java
public class BuilderMain
{
	public static void main(String[] args)
	{
		Director director = new Director();

		Builder builder = new ConcreteBuilder1();
		director.construct(builder);
		Product p1 = builder.getResult();
		p1.show();

		builder = new ConcreteBuilder2();
		director.construct(builder);
		Product p2 = builder.getResult();
		p2.show();
	}
}
```

总结：这个模式主要用在那些构建细节比较复杂，构建过程又比较单一，可以将这个过程总结出来，放到父类中强制子类实现；指挥官的角色会保证构建过程的完整。

