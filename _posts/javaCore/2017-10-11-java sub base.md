---
layout: default
title: java 子类修改基类属性
---

今天遇到一个问题，需要在基类中实现方法，子类中只需要修改一个基类的字段属性值，即可满足要求。  
试了几种不同方法，都是需要在子类中覆盖基类中的方法才能实现，这样的话，就会造成大量重复代码在项目中，前期代码如下：注意需要在`Sub`类中实现`print` 方法，如果有另外的类，`Sub2`, 还是要实现一遍print 方法，

```
public class Main
{
	public static void main(String[] args)
	{
		Base base = new Sub();
		base.print();
	}
}

class Base
{
	protected int a = 1;

	public void print()
	{
		System.out.println(this.a);
	}
}

class Sub extends Base
{
	protected int a = 2;

	public void print()
	{
		System.out.println(this.a);
	}
}
```


想了`n`久，问了各同事，才有了下面的代码，注意，不管如何继承，只需要设置 `a` 的值即可，这样，代码就能达到最简单，特别是对多个相同类的实现

```
public class Main
{
	public static void main(String[] args)
	{
		Base base = new Sub();
		base.print();
	}
}

class Base
{
	protected int a = 1;

	public void print()
	{
		System.out.println(this.a);
	}
}

class Sub extends Base
{
	{
		super.a = 2;
	}
}
```


# 总结

新的实现很简单，子类中直接修改了基类的属性，在调用基类方法时，实际使用的值，已被修改为子类值。  
以后可以有很多相同的可能性，都可能会使用到子类修改基类属性值