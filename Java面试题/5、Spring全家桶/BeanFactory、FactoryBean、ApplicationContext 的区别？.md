
BeanFactory 是 Spring IOC 容器的一个接口，用了获取 Bean 以及管理 Bean 的依赖注入和生命周期

FactoryBean 通常用于创建很复杂的对象，比如需要通过某种特定的创建过程才能得到的对象。

ApplicationContext 是 BeanFactory 的具体实现