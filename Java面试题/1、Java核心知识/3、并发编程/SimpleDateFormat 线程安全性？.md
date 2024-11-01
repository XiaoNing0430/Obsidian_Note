
SimpleDateFormat 是线程不安全的

# 线程不安全原因
SimpleDateFomat 中 format 方法在执行过程中，会使用一个成员变量 calendar 来保存时间。

如果在声明 SimpleDateFormat 的时候，使用的也是 static 定义的。那么这个 SimpleDateFotmat 就是一个共享变量，随之，SimpleDateFormat 中的 calendar 也就可以被多个线程访问到。

除了 format 方法之外，parse 方法也有同样的问题

不要把 SimpleDateFormat 作为一个共享变量使用

# 如何解决
几种比较常用的方法：
1. 使用局部变量
2. 加同步锁
3. 使用 ThreadLocal
4. 使用 DateTimeFormatter