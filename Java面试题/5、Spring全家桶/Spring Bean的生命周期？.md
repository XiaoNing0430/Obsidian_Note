
![[Pasted image 20240427100641.png]]
整个生命周期可以大致分为3个大的阶段，分别是：创建、使用、销毁。还可以进一步分为5个小的阶段：实例化、初始化、注册 Destruction 回调、Bean 的正常使用、Bean 的销毁

1. **实例化 Bean**
	- Spring 容器首先创建 Bean 实例
	- 在 <font color="orange">AbstractAutowireCapableBeanFactory</font> 类中的 **createBeanInstance** 方法中实现

2. **设置属性值**
	- Spring 容器注入必要的属性到 Bean 中
	- 在 <font color="orange">AbstractAutowireCapableBeanFactory</font> 的 **populateBean** 方法中处理

3. **检查 Aware**
	- 如果 Bean 实现了 BeanNameAware、BeanClassLoaderAware 等这些 Aware 接口，Spring 容器会调用它们
	- 在 <font color="orange">AbstractAutowireCapableBeanFactory</font> 的 **initializeBean** 方法中调用

4. **调用 BeanPostProcessor 的前置处理方法**
	- 在 Bean 初始化之前，允许自定义的 BeanPostProcessor 对 Bean 实例进行处理。如修改 Bean 的状态。BeanPostProcessor 的 postProcessBeforeInitialization 方法在此时被调用
	- 由 <font color="orange">AbstractAutowireCapableBeanFactory</font> 的 **applyBeanPostProcessorsBeforeInitialization** 方法

5. **调用 InitializingBean 的 afterPropertiesSet 方法**
	- 如果 Bean 实现了 InitializingBean 接口，afterPropertiesSet 方法在此时调用
	- 在 <font color="orange">AbstractAutowireCapableBeanFactory</font> 的 **invokeInitMethods** 方法中调用

6. **调用自定义的 init-method 方法**
	- 如果 Bean 定义了初始化方法，那么该方法会被调用
	- 在 <font color="orange">AbstractAutowireCapableBeanFactory</font> 的 **invokeInitMethods** 中调用

7. **调用 BeanPostProcessor 的后置处理方法**
	- 在 Bean 初始化之后，允许自定义的 BeanPostProcessor 对 Bean 实例进行处理。如修改 Bean 的状态。BeanPostProcessor 的 postProcessAfterInitialization 方法在此时被调用
	- 由 <font color="orange">AbstractAutowireCapableBeanFactory</font> 的 **applyBeanPostProcessorAfterInitialization** 方法

8. **注册 Destruction 回调**
	- 如果 Bean 实现了 DisposableBean 接口 或 在Bean定义中指定了自定义的销毁方法，Spring 容器会为这些 Bean 注册一个销毁回调，确保在容器关闭时能够正确地清理资源
	- 在 <font color="orange">AbstractAutowireCapableBeanFactory</font> 中 **registerDisposableBeanIfNecessary** 中实现

9. **Bean 准备就绪**
	- 此时，Bean 已完全准备就绪，可以处理应用程序的请求

10. **调用 DisposableBean 的 destory 方法**
	- 当容器关闭时，如果 Bean 实现了 DisposableBean 接口，destory 方法会被调用
	- 在 <font color="orange">DisposableBeanAdapter</font> 的 **destory** 方法中实现

11. **调用自定义的 destory-method**
	- 如果 Bean 在定义中指定了自定义的销毁方法，那么该方法会被调用
	- 在 <font color="orange">DisposableBeanAdapter</font> 的 **destory** 方法中实现