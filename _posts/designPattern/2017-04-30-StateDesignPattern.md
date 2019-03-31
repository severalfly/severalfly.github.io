---
layout: default
title: 状态模式
---


定义：当一个对象的内存状态改变时允许改变其行为，这个对象看起来像日改变了其烦类。  
主要解决的是当控制一个对象状态转换的条件表达式过于复杂时的情况。把状态的判断逻辑转移到表示不同状态的系列类当中，可以把复杂的判断逻辑简化。如果判断很简单，那就没必要了


例子说话

首先定义一个虚基类，定义当前任务的方法
```java
public abstract class State
{
	public abstract void writeProgram(Work w);
}
```


工作啦，有当前状态/当前时间/当前完成状况，此例子中，上午是不能完成任务的，
```java
public class Work
{
	private State current;
	private double hour;
	private boolean finish = false;

	public Work()
	{
		this.current = new ForennoState();
	}

	public double getHour()
	{
		return hour;
	}

	public void setHour(double hour)
	{
		this.hour = hour;
	}

	public boolean isFinish()
	{
		return finish;
	}

	public void setFinish(boolean finish)
	{
		this.finish = finish;
	}

	public void setState(State s)
	{
		this.current = s;
	}

	public void writeProgram()
	{
		this.current.writeProgram(this);
	}
}
```


起始状态，
```java
public class ForennoState extends State
{
	// 9点上班开始
	@Override
	public void writeProgram(Work w)
	{
		if (w.getHour() < 12)
		{
			System.out.println("当前时间" + w.getHour() + "点 上午工作，精神百倍");
		}
		else
		{
			w.setState(new NoonState());
			w.writeProgram();
		}
	}
}
```


一个中间状态
```java
public class NoonState extends State
{

	@Override
	public void writeProgram(Work w)
	{
		if (w.getHour() < 13)
		{
			System.out.println("当前时间" + w.getHour() + "点 饿了，午饭：犯困，午休");
		}
		else
		{
			w.setState(new AfternoonState());
			w.writeProgram();
		}
	}
}
```



另一个中间状态
```java
public class AfternoonState extends State
{

	@Override
	public void writeProgram(Work w)
	{
		if (w.getHour() <= 17)
		{
			System.out.println("当前时间" + w.getHour() + "点 下午状态还不错，纬线努力");
		}
		else
		{
			w.setState(new EvenintState());
			w.writeProgram();
		}
	}

}
```



还是一个中间状态
```java
public class EvenintState extends State
{

	@Override
	public void writeProgram(Work w)
	{
		if (w.isFinish())
		{
			w.setState(new RestState());
			w.writeProgram();
		}
		else
		{
			if (w.getHour() < 21)
			{
				System.out.println("当前时间" + w.getHour() + "点 加班哦，疲累之极");
			}
			else
			{
				w.setState(new SleepingState());
				w.writeProgram();
			}
		}
	}

}
```



一个结束状态
```java
public class RestState extends State
{

	@Override
	public void writeProgram(Work w)
	{
		System.out.println("当前时间" + w.getHour() + "点 下班");
	}

}
```



另一个结束状态
```java
public class SleepingState extends State
{

	@Override
	public void writeProgram(Work w)
	{
		System.out.println("当前时间" + w.getHour() + "点不行了，睡觉");
	}

}
```


客户端代码就简单了
```java
public class StateMain
{
	public static void main(String[] args)
	{
		Work w = new Work();
		w.setHour(9);
		w.writeProgram();

		w.setHour(10);
		w.writeProgram();

		w.setHour(12);
		w.writeProgram();

		w.setHour(13);
		w.writeProgram();
		w.setHour(14);
		w.writeProgram();

		w.setHour(17);
		w.writeProgram();
		w.setFinish(true);

		w.setHour(19);
		w.writeProgram();

		w.setHour(22);
		w.writeProgram();

	}
}
```

总结，此模式主要对大量判断当前状态时有效，将不同的状态分成不同的类去实现，让类之间去做下个类的交互，减轻客户端调用时的复杂度。客户端在使用时，只需要知道当前时间即可，其他就交给各个类去实现

