
java中 Stop The World 机制简称 STW，是在执行垃圾收集算法时，java应用程序的其他所有线程都被挂起。这是 java 中一种全局暂停现象，全局停顿，所有 java 代码停止，native 代码可以执行，但不能与 JVM 交互。
