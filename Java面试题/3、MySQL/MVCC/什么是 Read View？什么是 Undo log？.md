
# 什么是 Read View？

Read View 主要来帮助我们解决可见性问题的，会告诉我们本次事务应该看到哪个版本的数据。
- **m_trx_ids**：当前活跃事务列表
- **min_trx_id**：m_trx_ids 中最小的值
- **max_trx_id**：最大值，应该分配给下一个事务的 id
- **creator_trx_id**：当前事务id。每开启一个事务都会生成一个 Read View，creator_trx_id 就是这个开启的事务的id

# 什么是 Undo log？

Undo log 是 MySQL中比较重要的事务日志之一，Undo log 是一种用于回退的日志，在事务没提交之前，MySQL 会先记录更新前的数据到 Undo log 日志中，当前事务回滚或者数据库崩溃时，可以利用 Undo log 来进行回退


# 行记录的隐藏字段

- **db_row_id**：隐藏主键，如果没有给表创建主键，那么会以这个字段来创建聚簇索引
- **db_trx_id**：对这条数据做了最新一次修改的事务id
- **db_roll_ptr**：回滚指针，指令这条记录的上一个版本，其实指向的就是 Undo log 中的上一个版本的快照的地址

每次记录变更之前都会先存储一份快照到 Undo log 中，那么这几个隐式字段也会跟着记录一起保存在 undo log 中，每个快照中都有一个 db_trx_id 字段表示对这个记录做了最新一次修改的事务的id，以及一个 db_roll_ptr 字段指向了上一个快照的地址