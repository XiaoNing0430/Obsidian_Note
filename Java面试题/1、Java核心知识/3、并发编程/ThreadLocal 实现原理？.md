
每个线程都有自己的 ThreadLocalMap，其中 key 是 ThreadLocal 实例 是弱引用，value 是 set 进去的值 是强引用。

会有内存泄露的问题

ThreadLocalMap 的 value 是强引用，只要线程还存在 value 就不会被回收，会产生数据错乱、内存泄露的问题。

解决：使用完后，手动 remove()