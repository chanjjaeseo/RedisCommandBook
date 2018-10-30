### Redis 持久化

#### Redis数据存储方式

cache-only: 只当作缓存服务，宕机或关闭时数据不存在

persistence: 采用策略对redis做持久化操作

#### Redis持久化方式

Redis持久化：redis将所有数据保存在内存中，对数据的更新异步地保存在磁盘当中

1. 快照 RDB （Redis DataBase）
2. 写日志 AOF (Append-only file)

#### RDB持久化

RDB三种保存方式

RDB文件采用二进制文件的形式保存到硬盘中

触发机制

- save 同步命令 (会阻塞其他命令)

文件策略：如存在老的RDB文件，新的替换老的文件

复杂度: O(n)

- bgsave 异步命令

操作过程: fork()函数生成子进程去做RDB操作，fork()命令会阻塞redis主进程(这个动作一般很快完成)

文件策略：与save相同

复杂度: O(n)

- 自动

操作过程: 满足以上条件redis会自动做RDB操作(bgsave)

配置    seconds     changes

save    9000          1

save    300           10

save    60            10000

