
1. 字段注入
```java
@Autwired
private Bean bean;
```

2. 构造器注入
```java
@Component
class Test {
	private final Bean bean;

	@Autwired
	public Test(Bean bean) {
		this.bean = bean;
	}
}
```

3. setter 注入
```java
@Component
class Test {
	private final Bean bean;

	@Autwired
	public void setBean(Bean bean) {
		this.bean = bean;
	}
}
```