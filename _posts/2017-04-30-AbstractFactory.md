---
layout: default
title: 抽象工厂模式
---

![](http://dl2.iteye.com/upload/attachment/0091/6123/34023d11-556f-3377-b883-6820347e8ed3.png?_=3700820)

定义：提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类


通常是在运行时刻再创建一个`concreteFactory`类的实例，这个具体工厂再创建具有特定实现的产品对象，也就是说，为创建不同的产品对象，客户端应使用不同的具体工厂  
所有在用简单工厂的地方，都可以考虑用反射技术来去除`switch`或`if`，解除分支判断带来的耦合  
反射技术可以将简单工厂中的判断去除，进一步解放代码。

先看个例子，慢慢解释


先是一个用户表，可能会存在多个字段，这里只用id 与name 示例
```java
class User
{
	private int id;
	private String name;
	// getter && setter
}
```

用户表可能的操作，这里只示意插入与查询
```java

public interface IUser
{
	void insert(User user);
	User getUser(int id);
}

```

另外一个表，同用户表，这里用部门表示例做区分，用以扩展业务
```java
public class Department
{
	private String name;
	// getter && setter
}
```

部门表的不同操作
```java
public interface IDepartment
{
	void insert(Department department);
	Department getDepartment(String name);
}
```

定义不同数据库操作方法的接口
```java
interface IFactory
{
	IUser createUser();
}
```

具体数据库连接 `SQLServer`
```java
class SqlServerFactory implements IFactory
{
	public IUser createUser()
	{
		return new SqlServerUser();
	}
}
```


具体数据库连接 `Access`
```java
class AccessFactory implements IFactory
{
	public IUser createUser()
	{
		return new Accessuser();
	}
}
```


`SqlServer` 用户操作实现方法
```java
class SqlServerUser implements IUser
{
	public void insert(User user)
	{
		System.out.println("sql server insert user");
	}

	public User getUser(int id)
	{
		System.out.println("get user from sql server");
		return null;
	}
}

```

`SqlServer` 部门表的操作实现方法
```java
public class SqlServerDepartment implements IDepartment
{
	public void insert(Department department)
	{
		System.out.println("insert department to sqlserver");
	}
	public Department getDepartment(String name)
	{
		System.out.println("get department from sqlserver");
		return null;
	}
}

```


`Access` 用户操作方法
```java
class Accessuser implements IUser
{
	public void insert(User user)
	{
		System.out.println("access insert user");
	}
	public User getUser(int id)
	{
		System.out.println("get user from access");
		return null;
	}
}

```



`Access` 部门表的操作方法
```java
public class AccessDepartment implements IDepartment
{

	public void insert(Department department)
	{
		System.out.println("insert into access a department");
	}

	public Department getDepartment(String name)
	{
		System.out.println("get a department from access");
		return null;
	}

}
```



管理所有数据库连接的获取，这里用了两个方式  

1. `switch` 关键词直接判断，可用
2. 利用反射技术，此示例代码会抛出异常，因为配置的类的全路径需要修改，项目中是需要使用配置文件的，也就不会有此问题
```java
public class DataAccess
{
	private static db d = db.SqlServer;

	public static IUser createUser()
	{
		IUser result = null;
		try
		{
			result = (IUser) Class.forName(d.toString()).newInstance();
		}
		catch (Exception e)
		{
			e.printStackTrace();
		}
		if (result != null)
		{
			return result;
		}
		switch (d)
		{
		case SqlServer:
			result = new SqlServerUser();
			break;
		case Access:
			result = new Accessuser();
			break;
		default:
			break;
		}
		return result;
	}

	public static IDepartment createDepartment()
	{
		IDepartment result = null;
		try
		{
			result = (IDepartment) Class.forName(d.toString()).newInstance();
		}
		catch (Exception e)
		{
			e.printStackTrace();
		}
		if (result != null)
		{
			return result;
		}
		switch (d)
		{
		case SqlServer:
			result = new SqlServerDepartment();
			break;
		case Access:
			result = new AccessDepartment();
			break;
		default:
			break;
		}
		return result;

	}
}

enum db
{
	SqlServer, Access
}
```


客户端代码就比较简单了，此模式要实现的目的就是让客户端成为傻逼，只要做事就可以了，其他的由配置完成，客户端调用时完全无感，这样如果增加新的数据库，只要实现`IFactory`接口获取连接、实现`IUser`获取对用户表的操作、实现`IDepartment` 获取对部门表的操作就可以。  
没办法，改换数据库是个大工程，必须要有修改，我们要实现的目的就是将修改减至最少
```java
public class AbstractFactoryMain
{
	public static void main(String[] args)
	{
		User user = new User();
		Department dept = new Department();

		IUser iu = DataAccess.createUser();
		iu.insert(user);
		iu.getUser(1);

		IDepartment department = DataAccess.createDepartment();
		department.insert(dept);
		department.getDepartment("le");
	}
}

```


总结：此模式名为抽象工厂，区别与前面的工厂模式的就是这里的工厂方法也是抽象的，可以有多个不同的具体工厂
