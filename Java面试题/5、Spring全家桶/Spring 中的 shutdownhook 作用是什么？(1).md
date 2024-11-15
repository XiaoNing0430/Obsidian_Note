
在 Spring 框架中，Shutdown Hook（关闭钩子）是一种机制，用于在应用程序关闭时执行一些清理操作。

Spring 会向 JVM 注册一个 Shutdown Hook，在接收到关闭通知的时候，进行 Bean 的销毁，容器的销毁处理等操作。

在 Spring 框架中，可以使用 AbstractApplicationContext 类或其子类注册 Shutdown Hook。这些类提供了一个 registerShutdownHook() 方法，用于将 Shutdown Hook 与应用程序上下文关联起来

很多中间件的优雅上下线功能（优雅停机），都是基于 Spring 的 Shutdown Hook 机制实现的，比如 Dubbo 的优雅下线。

1. 实现 DisposableBean 接口，实现 destory 方法
2. 使用 @PreDestory 注解
3. 监听 ContextClosedEvent 事件