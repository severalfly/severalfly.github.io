---
layout: default
title: 原型模式
---

> 定义：用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象

这个主要用的是 `clone`，需要注意的是深复制/浅复制  
先上个例子

本模式用的是个简历，这是简历原型，实现 `Cloneable` 接口，内部实现clone 方法
```
class Resume implements Cloneable
{
	private String name;
	private String sex;
	private String age;
	private String timeArea;
	private String company;

	public Resume(String name)
	{
		super();
		this.name = name;
	}

	public void setPersonalInfo(String sex, String age)
	{
		this.sex = sex;
		this.age = age;
	}

	public void setWorkExperience(String timeArea, String company)
	{
		this.timeArea = timeArea;
		this.company = company;
	}

	public void show()
	{
		System.out.println(this.name + " " + this.sex + " " + this.age);
		System.out.println("工作经历 " + this.timeArea + " " + this.company);
	}

	public Resume clone()
	{
		try
		{
			return (Resume) super.clone();
		}
		catch (CloneNotSupportedException e)
		{
			e.printStackTrace();
		}
		return null;
	}
}

```

客户端代码就简单了，`r2 = r.clone()` 就可以创建新的对象了，而不是使用new，生产环境中，少使用new，因为new是比较消耗资源的
```
public class ResumeMain
{
	public static void main(String[] args)
	{
		Resume r = new Resume("leon");
		r.setPersonalInfo("男", "23");
		r.setWorkExperience("123", "huoli");
		r.show();

		Resume r2 = r.clone();
		r2.setPersonalInfo("女", "25");
		r2.setWorkExperience("severalfly", "thadsoifa");
		r2.show();
	}
}
```
直接复制，产生的新对象太快，注意会有浅复制的问题。下面来还原下


