
Spring 的 Bean 是否线程安全，这个要取决于他的作用于。Spring 的 Bean 有多种作用域，其中用的比较多的就是 **Singleton** 和 **Prototype**

默认情况下，Spring Bean 是单例的（Singleton）。这意味着在整个 Spring 容器中，只存在一个 Bean 实例。如果将 Bean 的作用域设置为原型的（Prototype），那么每次从容器中获取 Bean 时都会创建一个新的实例。

对于 Prototype 这种作用域的 Bean，他的 Bean 实例不会被多个线程共享，所以不存在线程安全的问题。

但是对于 Singleton 的 Bean，就可能存在线程安全问题了，但是也不绝对，要看这个 Bean 中是否有共享变量。