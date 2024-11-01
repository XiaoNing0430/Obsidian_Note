
1. **身份信息存储**
	- 在很多应用中，都需要做登录鉴权，一旦鉴权通过后，就可以把用户信息存储在 ThreadLocal 中，这样在后续的流程中，需要获取用户信息的，直接从 ThreadLocal 中获取即可

2. **线程安全**
	- ThreadLocal 可以用来定义一些需要并发安全处理的成员变量，比如 SimpleDateFormat，由于 SimpleDateFormat 不是线程安全的，可以使用 ThreadLocal 为每个线程创建一个独立的 SimpleDateFormat 实例，从而避免线程安全问题

3. **日志上下文存储**
	- 在 Log4j 等日志框架中，经常使用 ThreadLocal 来存储与当前线程相关的日志上下文。这允许开发者在日志消息中包含特定于线程的信息，如用户ID 或 事务ID，这用于调试和监控是非常有用的

4. **traceId 存储**
	- 在分布式链路追踪中，需要存储本次请求的 traceId，通常也是基于 ThreadLocal 存储的

5. **数据库 Session**
	- 很多 ORM 框架，如 Hibernate、MyBatis，都是使用 ThreadLocal 来存储和管理数据库会话的。这样可以确保每个线程都有自己的会话实例，避免了在多线程环境中出现的线程安全问题。

6. **PageHelper 分页**
	- PageHelper 是 MyBatis 中提供的分页插件，主要用来做物理分页的。我们在代码中设置的分页参数信息，页码和页大小等信息都会存储在 ThreadLocal 中，方便在执行分页时读取这些数据。