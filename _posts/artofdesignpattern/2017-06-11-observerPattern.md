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


