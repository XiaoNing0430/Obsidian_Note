
在 Java 中，实现动态代理有两种方式：
1. JDK动态代理
	- java.lang.reflect 包中的 Proxy 类和 InvocationHandler 接口提供了生成动态代理类的能力
2. Cglib动态代理
	- Cglib（Code Generation Library）是一个第三方代码生成类库，运行时在内存中动态生成一个子类对象从而实现对目标对象功能的扩展

# JDK 动态代理 和 Cglib 动态代理的区别

JDK 动态代理的对象必需**实现一个或多个接口**；使用 Cglib 代理的对象无需实现接口，达到代理类无侵入，可以在运行时**动态的生成某个类的子类**