
1. InnoDB 支持行级锁，MyISAM 不支持 行级锁
2. InnoDB 支持事务，MyISAM 不支持事务
3. InnoDB 支持聚簇索引，MyISAM 不支持聚簇索引
4. InnoDB 不存储行数，MyISAM 存储行数
5. InnoDB 支持外键，MyISAM 不支持外键
6. InnoDB 5.6之前不支持全文索引
7. 清空表时，InnoDB 是一行行删除，MyISAM 是重建表
8. 对于自增长字段，InnoDB 中必须包含只有该字段的索引，MyISAM 可以和其他字段建联合索引