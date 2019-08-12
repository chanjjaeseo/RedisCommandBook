## Redis 两种高可用方式

1. redis 主从 + 哨兵 (2.x支持)
2. redis cluster（集群模式）（3.x支持）

## Redis 主从哨兵

#### 原理

主节点负责写，从节点负责读，通过哨兵(sentinel)来发现和转移故障

一般的架构方式是一主两从，三个哨兵，当两个以上的哨兵认为主节点挂掉就开始重新选择主节点并转移故障节点

优点:
1. 搭建方式简单
2. 有故障转移的功能

缺点:
1. 容量和性能有限，有性能上的瓶颈
2. 不易水平拓展

#### Redis Leader 主节点配置配置

```
port 7000
daemonize yes
pidfile /var/run/redis-7000.pid
logfile "7000.log"
dir "/opt/soft/redis/data"
```

#### Redis Fllower 从节点配置

```
port 7001
daemonsize yes
pidfile /var/run/redis-7001.pid
logfile "7001.log"
dir "/opt/soft/redis/data"
slaveof 127.0.0.1 7000
```

#### Redis Sentinel 主要配置

```
port ${port}
dir "/opt/soft/redis/data"
logfile "${port}.log"
sentinel monitor mymaster 127.0.0.1 7000 2
sentinel down-after-millseconds mymaster 30000
sentinel parallel-syncs mymaster 1
sentinel failover-timeout mymaster 180000
```

## Redis Cluster 

#### Redis Cluster 配置

```
port 7000
daemonize yes
dir "/opt/soft/redis/data"
logfile "7000.log"
dbfilename "dump-7000.rdb"
cluster-enabled yes
cluster-config-file node-7000.conf
cluster-require-full-coverage no
```




