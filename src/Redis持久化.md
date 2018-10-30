### Redis 持久化

#### Redis数据存储方式

cache-only: 只当作缓存服务，宕机或关闭时数据不存在

persistence: 采用策略对redis做持久化操作

#### Redis持久化方式

Redis持久化：redis将所有数据保存在内存中，对数据的更新异步地保存在磁盘当中

1. 快照 RDB （Redis DataBase）
2. 写日志 AOF (Append-only file)

#### RDB持久化

#####RDB三种保存方式

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


#####RDB配置

dbfilename：持久化数据存储在本地的文件 

    dbfilename dump.rdb 

dir：持久化数据存储在本地的路径，如果是在/redis/redis-3.0.6/src下 启动的redis-cli，则数据会存储在当前src目录下 

    dir ./ 

snapshot触发的时机，save <seconds> <changes> ##如下为900秒后，至少有一个变更操作，才会snapshot ##对于此值的设置， 需要谨慎，评估系统的变更操作密集程度 ##可以通过save “”来关闭snapshot功能 #持久化(以快照的方式) 策略（默认） 

    save 900 1 （15分钟变更一次） 
        
    save 300 10 （5分钟变更10次） 
        
    save 60 10000 （1分钟变更1万次）


当snapshot时出现错误无法继续时，是否阻塞客户端“变更操作”， “错误”可能因为磁盘已满/磁盘故障/OS级别异常等 
    
    stop-writes-on-bgsave-error yes

是否启用rdb文件压缩，默认为“yes”，压缩往往意味着“额外的cpu消耗”， 同时也意味这较小的文件尺寸以及较短的网络传输时间 
    
    rdbcompression yes

RDB异常恢复

    > redis-check-rdb

#### AOF持久化

##### AOF保存命令的方式

1.将写命令刷新到缓冲区

2.缓冲区在将命令fsync刷新到硬盘


##### AOF的三种保存方式

AOF将每条命令依次写入到硬盘中，异常恢复时，只需要执行所有命令即可恢复

1.appendfsync always  每条命令都将缓冲区的命令数据刷新到硬盘

优点:   不丢数据

缺点： IO开销大，一盘的sata盘只有几百的TPS

2.(推荐)appendfsync  everysec  每秒将缓冲区的命令数据刷新到硬盘

优点：较always减少了IO的压力

缺点: 丢一秒数据

3.appendfsync no  缓冲区刷新的时机根据OS(操作系统)决定

优点：不用管

缺点：不可控

#### RDB和AOF到底该如何选择

（1）不要仅仅使用RDB，因为那样会导致你丢失很多数据
（2）也不要仅仅使用AOF，因为那样有两个问题
    
第一，你通过AOF做冷备，没有RDB做冷备，来的恢复速度更快; 

第二，RDB每次简单粗暴生成数据快照，更加健壮，可以避免AOF这种复杂的备份和恢复机制的bug

（3）综合使用AOF和RDB两种持久化机制，用AOF来保证数据不丢失，作为数据恢复的第一选择; 
用RDB来做不同程度的冷备，在AOF文件都丢失或损坏不可用的时候，还可以使用RDB来进行快速的数据恢复

