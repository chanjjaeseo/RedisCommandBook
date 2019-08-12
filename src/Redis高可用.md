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
