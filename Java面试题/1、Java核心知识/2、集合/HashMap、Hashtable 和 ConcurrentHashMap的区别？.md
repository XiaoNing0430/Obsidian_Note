
线程安全：
- HashMap 是线程不安全的
- Hashtable 中的方法是同步的，所以是线程安全的
- ConcurrentHashMap 
	- 在 jdk1.8 之前使用分段锁保证线程安全，ConcurrentHashMap 默认情况下将 Hash 表分为16个桶，在加锁时，针对每个单独的分片进行加锁，其他分片不受影响。锁的粒度更细，性能更好
	- 在 jdk1.8 中，采用了一种新的方式来实现线程安全，使用了 CAS + Synchronized，这个实现被称为 “分段锁” 的变种，也被称为 “锁分离”，将锁定粒度更细，把锁的粒度从整个 Map 降低到了单个桶

继承关系
- Hashtable 基于旧的 Dictionary 类继承来的
- HashMap 继承 抽象类AbstractMap 实现 Map 接口
- ConcurrentHashMap 继承 抽象类AbstractMap，实现了 ConcurrentMap 接口

允不允许null值
- Hashtable 中，key 和 value 都不允许出现 null 值，否则会抛出 NullPointerException
- HashMap 中，null 可以作为 key 或 value 都可以
- ConcurrentHashMap 中，key 和 value 都不允许 null 值