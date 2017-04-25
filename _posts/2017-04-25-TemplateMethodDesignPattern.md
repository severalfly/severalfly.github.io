---
layout: default
title: 模版模式
---

定义：一个操作中的算法的骨架，而将一些步骤延迟到子类中。模版方法可以使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤

还是上例子


首先定义的是父类，也就是定义了今天问题的模版，包含两个测试，每个测试包含题目与答案，在本例子中，答案是每个人的不同结果，所以这里将`answer1` 和 `answer2` 放到子类中去实现，
```java
public abstract class PaPerTest
{
	public void question1()
	{
		System.out.println("questionA");
		System.out.println("答案" + answer1());
	}

	public void question2()
	{
		System.out.println("questionB");
		System.out.println("答案" + answer2());
	}

	abstract public String answer1();

	abstract public String answer2();
}
```

第一个子类，只要给出两个问题的答案就可以了，
```java
public class PaPerTestA extends PaPerTest
{

	@Override
	public String answer1()
	{
		return "a";
	}

	@Override
	public String answer2()
	{
		return "b";
	}

}
```


第二个子类，同上，也只是给出答案即可
```java
public class PaperTestB extends PaPerTest
{

	@Override
	public String answer1()
	{
		return "c";
	}

	@Override
	public String answer2()
	{
		return "d";
	}

}
```


客户端调用代码，这里`PaPerTest a = new PaPerTestA();`将子类实例化到父类，也就调用了父类的方法；同时因为实例化的是不同的子类，`answer1` 和 `answer2` 也都是调用的对应的子类的实现，这样就可以把实现放到客户端调用时
```java
public class TemplateMain
{

	public static void main(String[] args)
	{
		PaPerTest a = new PaPerTestA();
		a.question1();
		a.question2();
		PaPerTest b = new PaperTestB();
		b.question1();
		b.question2();
	}
}
```

# 总结
模版模式也是将父类的方法放到子类中去实现，将类的实例化延迟到调用时，也就是面向对象编程的多态性，`JAVA`本来就是全虚类的，本身就支持这种延迟实例化

