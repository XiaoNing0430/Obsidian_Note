
- **错误日志（error log）**：对 MySQL 的启动、运行、关闭过程进行了记录
- **二进制日志（binary log）**：主要记录的是更改数据库数据的SQL语句
- **一般查询日志（general query log）**：已建立连接的客户端发送给MySQL服务器的所有SQL记录，因为SQL的量比较大，默认是不开启的，也不建议开启
- **慢查询日志（slow query log）**：执行时间超过 `long_query_time` 秒钟的查询，解决SQL慢查询问题的时候会用到
- **事务日志（redo log 和 Undo log）**：redo log 是重做日志；Undo log 是回滚日志
- **中继日志（relay log）**：relay log 是复制过程中产生的日志，很多方面都跟 binary log 差不多。不过，relay log 针对的是主从复制中的从库
- **DDL日志（metadata log）**：DDL语句执行的元数据操作