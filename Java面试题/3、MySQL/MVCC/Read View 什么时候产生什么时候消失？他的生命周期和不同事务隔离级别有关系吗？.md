
[[什么是 Read View？什么是 Undo log？]]

在 Read Committed 隔离级别中，每次 Select 都会产生新的 Read View

在 Repeatable Read 隔离级别中，第一次 Select 会产生 Read View，只有在本事务中对数据进行更改或者加锁读时，会更新 Read View