
- Synchronized 是 Java内置特性；ReentrantLock是通过 Java代码实现的
- Synchronized 是可以自动获取/释放锁的；ReentrantLock 需要手动获取/释放锁
- ReentrantLock 具有响应中断、超时等特性
- ReentrantLock 可以实现公平锁和非公平锁；Synchronized 只是非公平锁