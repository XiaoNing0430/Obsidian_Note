
# Synchronized 如何使用？

## 同步方法
```java
// 同步方法，对象锁
public synchronized void doSth1() {
	System.out.println("Hello World");
}

// 同步方法，类锁
public synchronized static void doSth2() {
	System.out.println("Hello World");
}
```

## 同步代码块
```java
// 同步方法，类锁
public void doSth1() {
	synchronized(Dmeo.class) {
		System.out.println("Hello World");
	}
}

// 同步方法，对象锁
public void doSth2() {
	synchronized(this) {
		System.out.println("Hello World");
	}
}
```


# 加在普通方法上和加在静态方法的区别？
```java
// 同步方法，对象锁
public synchronized void doSth1() {
	System.out.println("Hello World");
}

// 同步方法，类锁
public synchronized static void doSth2() {
	System.out.println("Hello World");
}
```

Synchronized 的普通方法，锁的是具体调用这个方法的实例对象，Synchronized的静态方法，锁的是这个方法所属于的类对象
