
List 主要有 ArrayList、LinkedList 和 Vector 几种实现。这三者都实现了 List 接口，使用方式也很相似，主要区别在于因为实现的方式不同，所以对不同的操作具有不同的效率。

**ArrayList 是一个可改变大小的数组**。当更多的元素加入到 ArrayList 中时，其大小将会动态的增长，内部的元素可以直接通过 get 与 set 方法进行访问，因为 ArrayList 本质上就是一个数组

**LinkedList 是一个双向链表**，在添加和删除元素时具有比 ArrayList 更好的性能，但在 get 和 set 方面弱于 ArrayList。

**Vector 属于强同步类**。

Vector 扩容时每次请求其大小的双倍空间，而 ArrayList 扩容时每次对 size 增长 50%

LinkedList 还实现了 Queue 和 Deque 接口，该接口比 List 提供了更多的方法，包括 offer()、peek()、poll()