
volatile 在线程安全方面，可以保证有序性和可见性，但是不能保证原子性的。

Synchronized 可以保证原子性，是因为被 Synchronized 修饰的代码片段，在进入之前加了锁，只要没执行完，其他线程是无法获得锁执行这段代码片段的，就可以保证他内部的代码可以全部被执行，进而保证了原子性

**volatile 不能保证原子性，是因为它不是锁，没做任何可以保证原子性的操作。当然就不能保证原子性了**
