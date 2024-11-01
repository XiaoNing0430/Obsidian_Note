
# 什么是 Spring 的三级缓存？

在 Spring 的 BeanFactory 体系中，BeanFactory 是 Spring IOC 容器的基础接口，其 DefaultSingletonBeanRegistry 类 实现了 BeanFactory 接口，并维护了三级缓存：
```java
public class DefaultSingletonBeanRegistry extends SimpleAliasRegistry implements SingletonBeanRegistry {  
	// 一级缓存：保存完整的Bean对象
    private final Map<String, Object> singletonObjects = new ConcurrentHashMap(256);
    // 三级缓存：保存单例Bean的创建对象
    private final Map<String, ObjectFactory<?>> singletonFactories = new HashMap(16);  
    // 二级缓存：存储 “半成品” 的Bean对象
    private final Map<String, Object> earlySingletonObjects = new ConcurrentHashMap(16);
}
```
- **singletonObjects 是一级缓存，存储的是完整创建好的对象**
	- 在创建一个单例Bean时，会先从 singletonObjects 中尝试获取该Bean的实例，如果能够获取到，则直接返回该实例，否则继续创建该Bean

- **earlySingletonObjects 二级缓存，存储的是尚未完全创建好的单例Bean对象**
	- 在创建单例Bean时，如果发现该Bean存在循环依赖，会先创建该Bean的 “半成品” 对象，并将 “半成品” 对象存储到 earlySingletonObjects 中。当循环依赖的Bean创建完成后，Spring 会将完成的Bean实例对象存储到 singletonObjects 中

- **singletonFactories 是三级缓存，存储的是单例Bean的创建工厂**
	- 当一个单例Bean被创建时，Spring 会先将该Bean的创建工厂存储到 singletonFactories 中，然后再执行创建工厂的 getObject() 方法，生成该Bean的实例对象。在该Bean被其他Bean引用时，Spring会从 singletonFactories 中获取该Bean的创建工厂，创建出该Bean的实例对象，并将该Bean的实例对象存储到 singletonObjects 中


# 如何解决的循环依赖？

Spring 中 Bean的创建过程可以分为两步，第一步叫做实例化，第二步叫做初始化。

实例化的过程只需要调用构造方法吧对象创建出来并给他分配内存空间，而初始化则是给对象的属性进行赋值。

Spring之所以可以解决循环依赖就是因为对象的初始化是可以延后的，也就是说，当我创建一个Bean ServiceA 的时候，会先把这个对象实例化出来，然后再初始化其中的 serviceB 属性
```java
@Service
public class ServiceA {

	@Autowired
	private ServiceB serviceB;

}

@Service
public class ServiceB {

	@Autowired
	private ServiceA serviceA;

}
```
而当一个对象只进行了实例化，但是还没有进行初始化，我们称之为半成品对象。所以，所谓半成品对象，其实只是 Bean 对象的一个空壳子，还没进行属性注入和初始化。

当这两个Bean在初始化过程中相互依赖的时候，如初始化A发现他依赖了B，继续去初始化B，发现他又依赖了A，解决依赖流程如下：
![[Pasted image 20240427112008.png]]