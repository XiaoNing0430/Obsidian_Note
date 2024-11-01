
- **type**：表示查询时所使用的索引类型
	- system：系统表，少量数据，往往不需要磁盘io
	- const：使用常数索引，MySQL只会在查询时使用常数值进行匹配
	- eq_ref：唯一索引扫描，只会扫描索引树中的一个匹配行
	- ref：非唯一索引扫描，只会扫描索引树中的一部分来查找匹配的行
	- range：范围扫描，只会扫描索引树中的一个范围来查找匹配的行
	- index：全索引扫描，会遍历索引树来查找匹配行
	- all：全表扫描，将遍历全表来找到匹配的行
	 **system > const > eq_ref > ref > range > index > all**
	 
- **possible_keys**：表示可能被查询优化器选择使用的索引
- **key**：表示查询优化器选择使用的索引
- **Extra**：表示其他额外的信息，包括 Using Index、Using filesort、Using temporary等
	- Using where：表示MySQL将在存储引擎检索行后，在进行条件过滤（使用where子句）；查询的列为被索引覆盖，where筛选条件非索引的前导列或者where筛选条件非索引列
	- Using index：表示MySQL使用了索引覆盖优化，只需要扫描索引，而无需回到数据表中检索行
	- Using index condition：表示查询在索引上执行了部分条件过滤
	- Using where;Using index：查询的列被索引覆盖，并且where筛选条件是索引列之一，但不是索引的前导列，或者where筛选索引是索引列前导列的一个范围
	- Using join buffer：表示MySQL使用了连接缓存
	- Using temporary：表示MySQL创建了临时表来存储查询结果。通常是在排序或分组时发生的
	- Using filesort：表示MySQL将使用文件排序而不是索引排序，这通常发生在无法使用索引来进行排序时