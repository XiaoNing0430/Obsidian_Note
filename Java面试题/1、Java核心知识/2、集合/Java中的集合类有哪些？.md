
![[Pasted image 20240510094443.png]]

Java中的集合类，主要分为 List、Set、Queue、Stack、Map 等五种数据结构。其中，前四种数据结构都是单一元素的集合，而最后的 Map 则是以 KV 对的形式使用

从继承关系上讲，List、Set、Queue 都是 Collection 的子接口，Collection 又继承了 Iterable 接口，说明这几种集合都是可以遍历的。

从功能上讲，List 代表一个容器，可以先进先出，也可以先进后出。Set 相对于 List 来说，是无序的，也是一个去重的列表。Map 则是 KV 的映射，也会涉及到 key 值的查询能力

从实现上讲，List 可以有链表或者数组实现，链表增删快，数组查询快。Queue则可以分为优先队列，双端队列等。Map 则可以分为普通的 HashMap 和 可以排序的 TreeMap 等