#### Redis的五大数据结构

- string 字符串
- hash 哈希
- list 列表
- set 集合
- zset 有序集合

#### 常用命令

- keys [pattern]  遍历所有key （生产环境勿用，时间复杂度O(n)）
- dbsize  计算key的总数 (redis内置计数器，时间复杂度O(1))
- exists [key] 检查key是否存在 (时间复杂度O(1))
- del [key] 删除指定键值对 (时间复杂度O(1))
- expire [key] [seconds] 设置key过期时间 (时间复杂度O(1))
- ttl [key] 查看key过期时间 (时间复杂度O(1))
- type [key] 返回key的类型 (时间复杂度O(1))

#### 数据结构和内部编码

- string
  - raw
  - int
  - embstr
- hash
  - hashtable
  - ziplist
- list
  - linkedlist
  - ziplist
- set
  - hashtable
  - intset
- zset
  - skiplist
  - ziplist
  
#### Redis Object
  
  - 数据类型(type)
    - string
    - hash
    - list
    - set
    - sorted set
  - 编码方式(encoding)
    - raw
    - int
    - ziplist
    - linkedlist
    - hashmap
    - intset
  - 数据指针(ptr)
  - 虚拟内存(vm)
  - 其他信息
  
#### Hash 
  
  - 数据结构
    - [key] [field] [value]
  - 命令
    - hset [key] [field] [value] 新增hash
    - hget [key] [field] 获取key下的某个field的值
    - hdel [key] [field] 删除hash key的某个field
    - hgetall [key] 获取hash key对应的多有信息
    - hexists [key] [field] 判断key是否有field
    - hlen key 获取key下field的数量
    - hvals [key] 获取hash key所有value
    - hkeys [key] 获取hash key下的所有field
    - hmget [key] [field1] [field2] [field3] ... [fieldN] 获取批量key下field的信息
    - hmset [key] [field1] [value1] [field2] [value2] [field3] [value3] ...[fieldN][valueN] 批量新增hash
    - hincrby/hdecrby/hincr/hdecr [key] [field]
       
#### List

特点: 有序、可重复



    
   
