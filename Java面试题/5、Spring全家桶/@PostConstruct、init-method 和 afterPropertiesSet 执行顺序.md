
在 Spring 框架中，使用 @PostConstruct、自定义的 init-method 方法、InitializingBean 接口和 afterPropertiesSet 方法都是用于在 Bean初始化阶段执行特定方法的方式。他们的执行顺序是：**构造方法 > @PostConstruct > afterPropertiesSet > init-method**

- @PostConstruct
	- 是 java.annotation 包中的注解（Spring Boot 3.0 之后 jakarta.annotation）中，用于在构造函数执行完毕并且依赖注入完成后执行特定的初始化方法。标注在方法上表示这个方法将在Bean初始化阶段被调用

- afterPropertiesSet
	- 通过在 Bean 的配置中指定 init-method 属性，可以告诉 Spring 在Bean初始化完成后调用指定的初始化方法。

- init-method
	- 是 Spring 的 InitializingBean 接口中的方法。如果一个 Bean 实现了 InitializingBean 接口，Spring 在初始化阶段会调用该接口的 afterPropertiesSet 方法