
1. **工厂模式**：Spring 容器本质是一个大工厂，使用工厂模式通过 BeanFactory、ApplicationContext 创建 bean 对象。
    
2. **代理模式**：Spring AOP 功能功能就是通过代理模式来实现的，分为动态代理和静态代理。
    
3. **单例模式**：Spring 中的 Bean 默认都是单例的，这样有利于容器对Bean的管理。
    
4. **模板模式** ：Spring 中 JdbcTemplate、RestTemplate 等以 Template结尾的对数据库、网络等等进行操作的模板类，就使用到了模板模式。
    
5. **观察者模式**：Spring 事件驱动模型就是观察者模式很经典的一个应用。
    
6. **适配器模式** ：Spring AOP 的增强或通知 (Advice) 使用到了适配器模式、Spring MVC 中也是用到了适配器模式适配 Controller。
    
7. **策略模式**：Spring中有一个Resource接口，它的不同实现类，会根据不同的策略去访问资源。

8. **组合模式**：组合模式在 Spring MVC 中用的非常多，其中参数解析、响应值处理等模块就是使用了组合模式。
	拿参数解析模块举例：
	![[Pasted image 20240427152856.png]]
	 可以发现，整个参数解析模块中，由一个接口 HandlerMethodArgumentResolver 负责。其中父节点会实现该接口，同时对所有具体的子接口进行聚合。

9. **责任链模式**：对于 Spring MVC 来说，通过一系列的拦截器来处理请求执行前，执行后，以及结束的 response，核心的类是 `handlerExecutionChain`，它封装了 `HandlerAdapter` 和 一系列的过滤器

1. 