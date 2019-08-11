#### Redis 安装

- wget http://download.redis.io/releases/redis-3.0.7.tar.gz
- tar -xzf redis-3.07.tar.gz
- In -s redis-3.0.7 redis
- cd redis
- make && make install

#### Redis 可执行文件

- redis-server  | redis服务器 
- redis-cli | redis命令行客户端
- redis-benchmark  | redis 性能测试
- redis-check-aof | AOF文件修复
- redis-check-dump | RDB文件检查
- redis-sentinel | Sentinel服务器

#### Redis 安装验证

- ps -ef | grep redis
- netstat -antp | grep redis
- redis-cli -h ip -p port ping

#### Redis 的三种启动方式

- redis-server
- reids-server --port 6380
- redis-server configPath

#### Redis 常用配置

- deamonize  是否以守护进程的方式启动
- port Redis 对外端口
- logfile Redis 系统日志名称
- dir  Redis 工作目录（存放日志，持久化文件等）
