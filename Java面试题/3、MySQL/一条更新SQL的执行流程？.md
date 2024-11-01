
一次 InnoDB 的 update 操作，涉及到了 buffer pool、bin log、redo log 以及 物理磁盘，完整的一次操作过程基本如下：
1. 在 buffer pool 中读取数据。
	- 当 InnoDB 需要更新一条记录时，首先会在 buffer pool 中查找该记录是否在内存中。如果没有在内存中，则从磁盘读取该页到 buffer pool 中
2. 记录 Undo log
	- 在修改操作执行，InnoDB 会在 undo log 中记录修改前的数据。undo log 的写入最开始写到内存中，然后由1个后台线程定时刷新到磁盘中
3. 在 buffer pool 中更新
	- 当执行 update 语句时，InnoDB 会先更新已经读取到 buffer pool 中的数据，而不是直接吸入磁盘。同时，InnoDB 会将修改后的数据页状态设置为 “脏页” （Dirty Page）状态，表示该页已经被修改但尚未写入磁盘
4. 记录 redo log buffer
	- InnoDB 在 buffer pool 中记录修改操作的同时，InnoDB 会先将修改操作写入到 redo log buffer 中
5. 提交事务
	- 在执行完所有修改操作后，事务被提交。在提交时，InnoDB 会将 redo log 写入磁盘，以保证事务持久性
6. 写入磁盘
	- 在提交过程后，InnoDB 会将 buffer pool 中的脏页写入到磁盘，以保证数据的持久性。但是这个过程并不是立即执行的，是有一个后台线程异步执行的，所以可能会延迟写入，总之就是 MySQL 会选择合适的时机把数据写入磁盘做持久化
7. 记录 bin log
	- 在提交过程中，InnoDB 会将数据提交的信息记录到 bin log 中。

![[Pasted image 20240421112459.png]]