
[[什么是 Read View？什么是 Undo log？]]

用 Read View 和 Undo log 中每一个快照的 事务id比较：

- 如果 db_trx_id < min_trx_id，说明产生这条 Undo log的事务在 Read View 产生之前已经提交，可读
- 如果 db_trx_id =  creator_trx_id，说明产值这条 Undo log的事务 和 创建Read View 的事务是一个事务，可读
- 如果 min_trx_id < db_trx_id < max_trx_id 且 db_trx_id 在 m_trx_ids 列表中，说明在当前事务开启时，并未提交的事务在修改数据后提交了，不可读
- 如果 min_trx_id < db_trx_id < max_trx_id 且 db_trx_id 不在 m_trx_ids 列表中，说明在当前事务开启前，其他事务对数据进行修改并提交了，可见
- 如果 db_trx_id > max_trx_id，说明产生这条 Undo log 的事务在 Read View 产生时刻还没有开启，不可读

如果最终判断这条 Undo log 对 Read View 来说不可读，会根据 Undo log 的 db_roll_ptr 找到下一条 Undo log 进行比较，直到找到一个 Read View 可见的。