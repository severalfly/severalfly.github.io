---
layout: default
title: java 并发编程实战
---

# final 使用
此关键字使用过程中需要注意的问题，引用不可变，值可变；如果是基本类型，如`int`，则值也不可变
```

		FinalChangeObject obj = new FinalChangeObject();
		obj.setC(1);

		final FinalChangeObject t = obj;
		obj.setC(2);
		System.out.println(t.getC());

		FinalChangeObject obj2 = new FinalChangeObject();
		//		t = obj2;  //  这里就会报错，t 已经初始化引用到obj了，不能再改变其引用
		// 但是：对象的值是可以修改的，如上面的 obj.setC(2);
```



# 组合使线程安全

> 当为现有的类添加一个原子操作时，有一种更好的文法：组合（compostion）。通过自身的内置锁增加了一层额外的锁，它并不关心底层的list 是否是线程安全的，即使list 不是线程安全的或者修改了它的加锁实现，ImprovedList 也会提供一致的加锁机制来实现线程安全性。虽然额外的同步层可能导致轻微的性能损失，但与模拟另一个对象的加锁策略相比，ImporovedList 更为健壮。事实上，我们使用了java 监视器模式来封装现有的List，并且只要在类中拥有指向底层List 的唯一外部引用，就能确保线程安全性

