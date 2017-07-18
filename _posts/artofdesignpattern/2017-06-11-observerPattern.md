---
layout: default
title: 观察者模式
---

这是一个很棒的模式，下面用一个简单的Java 基础支持的例子来说明下

首先是观察者类，继承的是 `java.util.Observer` 类，成为一个具体的观察者
```
public class Player implements Observer
{
	@Override
	public void update(Observable o, Object arg)
	{
		System.out.println("player update");
		// 有更新操作时，通知其他观察者。 
		o.notifyObservers();
	}

}
```


// 具体的被观察类，这里只继承 `java.util.Observable`，里的方法已经足够使用，就没有做任何操作
```
public class ContreteObserable extends Observable
{

}
```


// 客户端，
```
public class ObJClient
{
	public static void main(String[] args)
	{
		// 先声明一个被观察者类
		Observable obserable = new ContreteObserable();
		// 再声明一个观察者类
		Observer player = new Player();
		// 将观察类加入到被观察者类的列表中，以便通知时可以通知到此观察者，也就是这里的player
		obserable.addObserver(player);

		// 有更新操作时
		player.update(obserable, new Object());
	}
}
```


上面使用`JDK`自带的观察者模式是存在问题的，最新使用方法如下：
首先还是定义一个观察者，继承自jdk 的`Observer`接口，实现其方法；实现很简单，打印出字符串表示收到更新操作，即可
```
public class Player implements Observer
{

	@Override
	public void update(Observable o, Object arg)
	{
		System.out.println("player update " + ((ContreteObserable) o).getData());
		// 有更新操作时，通知其他观察者。 
	}

}
```


然后是与前一种方法相差很大的重点，定义具体目标类，继承自`Observable`类，又增加了自己的属性表示数据可能存在变化，  
数据变化时，调用父类方法，通知监控数据已修改，再调用通知观察者的方法，即可实现通知
```
public class ContreteObserable extends Observable
{
	// 仅仅一个数据而已，用于标志目标是否变化，也可以用其他任何对象表示
	private String data = "";

	public String getData()
	{
		return this.data;
	}

	public void setData(String data)
	{
		if (!this.data.equalsIgnoreCase(data))
		{
			this.data = data;
			setChanged();
			notifyObservers();
		}
	}

}
```


客户端调用也有变化，定义一两个观察者，以示区分；  
然后目标类修改自身数据，强制变化，达到通知的目的
```
public class ObJClient
{
	public static void main(String[] args)
	{
		ContreteObserable obserable = new ContreteObserable();
		Observer player = new Player();
		obserable.addObserver(player);
		Observer player2 = new Player();
		obserable.addObserver(player2);
		obserable.setData("12");// 通知所有观察者，数据有变化了
	}
}
```

