---
layout: default
title: 备忘录模式
---

![](http://oou15cuq6.bkt.clouddn.com//image/pattern/%E5%A4%87%E5%BF%98%E5%BD%95_meitu_1.jpg)

定义：在不破坏封装性的前提下，捕获一个对象的内部状态，并在对象之外保存这个状态，这样以后就可将该对象恢复到原先保存的状态  
目测这个模式可以用在一分钟实现延迟消息任务的代码中

还是来个例子说明问题，此例子是游戏中的保存当前状态


首先定义游戏类，包含保存状态与恢复状态方法
```java
public class GameRole
{
	private int vit;// 生命力
	private int atk;// 攻击力
	private int def;// 防御力

	public GameRole(int vit, int atk, int def)
	{
		super();
		this.vit = vit;
		this.atk = atk;
		this.def = def;
	}

	public void show()
	{
		System.out.println("生命力" + this.vit);
		System.out.println("攻击力" + this.atk);
		System.out.println("防御力" + this.def);
	}

	/**
	 * 模拟战斗，全部力量规0
	 */
	public void fight()
	{
		this.vit = 0;
		this.atk = 0;
		this.def = 0;
	}

	public RoleStateMemento saveState()
	{
		return new RoleStateMemento(this.vit, this.atk, this.def);
	}

	public void recoverState(RoleStateMemento memento)
	{
		this.vit = memento.getVit();
		this.atk = memento.getAtk();
		this.def = memento.getDef();
	}

	// getters && setters	
}

```

角色状态保存的实体类，这个如果能继承角色类就更好了
```java

public class RoleStateMemento
{
	private int vit;// 生命力
	private int atk;// 攻击力
	private int def;// 防御力

	public RoleStateMemento(int vit, int atk, int def)
	{
		super();
		this.vit = vit;
		this.atk = atk;
		this.def = def;
	}
	// getters && setters
}
```

保存控制器
```java
public class RoleStateCaretaker
{
	private RoleStateMemento memento;

	public RoleStateMemento getMemento()
	{
		return memento;
	}

	public void setMemento(RoleStateMemento memento)
	{
		this.memento = memento;
	}

}
```

客户端调用方法，中间的`role.saveState()`创建了一个保存的实体类，放到`cartaker` 中，这样就在游戏外面保存了游戏状态，与正常业务（游戏）就不区分了，不影响正常功能
```java
public class StateMain
{
	public static void main(String[] args)
	{
		GameRole role = new GameRole(1, 20, 400);
		role.show();

		//System.out.println();
		RoleStateCaretaker caretaker = new RoleStateCaretaker();
		caretaker.setMemento(role.saveState());

		role.fight();
		role.show();

		//System.out.println();
		role.recoverState(caretaker.getMemento());
		role.show();
	}
}
```

总结：这个模式主要是应用部分，谨慎使用，会消耗内存，

