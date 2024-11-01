
AbstractQueuedSynchronizer（抽象队列同步器）出现在 jdk1.5 中。AQS 是很多同步器的基础框架，比如 ReentrantLock、CountDownLatch、Semaphore 等都是基于 AQS 实现的。除此之外，我们还可以基于 AQS，定制出我们所需要的同步器。

```java
public abstract class AbstractQueuedSynchronizer  
    extends AbstractOwnableSynchronizer  
    implements java.io.Serializable {}
```
![[Pasted image 20240424204406.png]]
在 AQS  内部，维护了一个 FIFO 队列和一个 volatile 的 int 类型的 state 变量。在 state = 1时，表示当前对象锁已经被占有了，state 变量的值修改动作是通过 CAS 来完成的。

FIFO 队列用来实现多线程的排队工作，当线程加锁失败时，该线程会被封装成一个 Node 节点来置于队列尾部。
![[Pasted image 20240424204629.png]]
当持有锁的线程释放锁时，AQS 会将等待队列中的第一个线程唤醒，并让其重新尝试获取锁。
![[Pasted image 20240424204713.png]]
	上图展示的是一个非公平锁，如果公平锁的第一步只进行判断队列中是否有前序节点，如果有的话，直接入队列，不会进行第一次的 CAS

# 同步状态 —— state
AQS 使用 一个 **volatile int** 类型的成员变量来表示同步状态，在 state = 1 的时候表示当前对象锁已经被占有了。它提供了三个基本方法来操作同步状态：getState()、setState(int newState)、compareAndSetState(int expect, int update)。这些方法允许在不同的同步实现中自定义资源的共享和独占方式。
```java
   /**  
	* 同步状态
	*/
	private volatile int state;  
  
   /**  
	* 获取同步状态
	*/
	protected final int getState() {  
		return state;  
	}  
  
   /**  
    * 获取同步状态
    */
	protected final void setState(int newState) {  
		state = newState;  
	}  
  
   /**  
    * CAS 更新状态
    */
	protected final boolean compareAndSetState(int expect, int update) {  
		// See below for intrinsics setup to support this  
		return unsafe.compareAndSwapInt(this, stateOffset, expect, update);  
	}
```

# FIFO队列 —— Node
AQS 内部通过一个内部类 —— Node，AQS 就是借助他来实现同步队列功能的。
![[Pasted image 20240424205211.png]]
当线程尝试获取资源失败时，AQS 会将该线程包装成一个 Node 节点，并将其插入同步队列的尾部。在资源可用时，队列头部的节点会尝试再次获取资源。（在 AQS 中，Node 也用于构建条件队列。当线程需要等待某个条件时，它会被加入到条件队列中。当条件满足时，线程会被转移会同步队列）
```java
// Node类用于构建对垒
static final class Node {  
    /** 表示节点正在共享模式下等待的标记 */  
    static final Node SHARED = new Node();  
    /** 表示节点正在以独占模式等待的标记 */  
    static final Node EXCLUSIVE = null;  
  
    /** waitStatus 值表示线程已取消 */  
    static final int CANCELLED =  1;  
    /** waitStatus 值表示后继线程需要取消停放 */  
    static final int SIGNAL    = -1;  
    /** waitStatus 值表示线程正在等待条件 */  
    static final int CONDITION = -2;
    /** waitStatus 值表示下一个 acquireShared 应无条件传播 */
    static final int PROPAGATE = -3;  
    /** 标记节点状态 */
	volatile int waitStatus;

	/** 前驱节点 */
	volatile Node prev;
	/** 后继节点 */  
	volatile Node next;
	/** 节点中的线程，存储线程引用，指向当前节点所代表的线程 */
	volatile Thread thread;
	Node nextWaiter;
	final boolean isShared() {  
        return nextWaiter == SHARED;  
    }
  
	final Node predecessor() throws NullPointerException {  
        Node p = prev;  
        if (p == null)  
            throw new NullPointerException();  
        else  
            return p;  
    }  
  
    Node() { 
    }  
  
    Node(Thread thread, Node mode) {
        this.nextWaiter = mode;  
        this.thread = thread;  
    }  
  
    Node(Thread thread, int waitStatus) {  
        this.waitStatus = waitStatus;  
        this.thread = thread;  
    }  
}  

/** 队列头节点，延迟初始化。只在 setHead 时修改 */
private transient volatile Node head;  

/** 队列尾节点，延迟初始化 */
private transient volatile Node tail;

/** 入队操作 */
private Node enq(final Node node) {
    for (;;) {
        Node t = tail;  
        if (t == null) { // 必需先初始化
            if (compareAndSetHead(new Node()))  
                tail = head;  
        } else {  
            node.prev = t;  
            if (compareAndSetTail(t, node)) {  
                t.next = node;  
                return t;  
            }  
        }  
    }  
}
```
就这样，一个又一个 Node 被连接在一起，就成为了一个 FIFO队列
![[Pasted image 20240424205453.png]]
	AQS 中的阻塞队列是一个 CLH 队列。CLH（Craig，Landin，and Hagersten）队列是一种用于实现自旋锁的有效数据结构。它是由 Craig，Landin，和 Hagersten 首次提出，因此得名