
# Redo log 怎么实现持久化？
以一个更新事务为例，redo log流转过程，如下所示？
![[Pasted image 20240415194440.png]]redo 流程：
1. 先将原始数据从磁盘中读入内存来，修改数据的内存拷贝
2. 生成一条 redo log 并写入 redo log buffer，记录的是数据被修改后的值
3. 当事务 commit 时，将 redo log buffer 中的内容刷新到 redo log file 中，对 redo log file 采用追加写的方式
4. 定期将内存中修改的数据刷新到磁盘中

# redo log 记录的什么信息？
存储的修改后的数据