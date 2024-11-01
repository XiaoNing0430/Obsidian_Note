
Redis 的内存淘汰策略用于在内存满了之后，决定哪些 key 要被删除。Redis 支持多种内存淘汰策略，可以通过配置文件中的 **maxmemory-policy** 参数来指定

支持的内存淘汰策略如下：
- **noeviction**：不会淘汰任何键值对，而是直接返回错误信息
- **allkeys-lru**：从所有 key 中选择最近最少使用的那个 key 并删除
- **volatile-lru**：从设置了过期时间的 key 中选择最近最少使用的 key 并删除
- **allkeys-random**：从所有 key 中随机选择一个 key 并删除
- **volatile-random**：从设置了过期时间的 key 中随机选择一个 key 并删除
- **volatile-ttl**：从设置了过期时间的 key 中选择剩余时间最短的 key 并删除
- **volatile-lfu**：从设置了过期时间的 key 中选择访问频率最低的那个删除
- **allkeys-lfu**：从所有 key 中选择访问频率最低的那个删除

当 Redis 作为缓存使用的时候，推荐使用 **allkeys-lru** 淘汰策略。该策略会将最近最少使用的 key 淘汰。默认情况下，使用频率最低则后期命中的概率也最低，所以删除

当 Redis 作为半缓存半持久化使用时，可以使用 volatile-lru。但因为 Redis 本身不建议保存持久化数据，所以只作为备选方案