###Redis笔记  
  <br>
   

Redis(Remote Dictionary Server)是用C语言开发的一个开源的高性能键值对(key-value)数据库。

---
####Redis特点
- 数据间没有必然的联系
- 内部采用单线程机制进行工作
- 高性能
- 多数据类型支持
    1. 字符串类型 string
    2. 列表类型 list
    3. 散列类型 hash
    4. 集合类型 set
    5. 有序集合类型 zset
- 持久化支持。可以进行数据灾难恢复

<br>
####Redis基本操作  
<br>

**1. 数据添加**
- 功能：设置key,value数据
- 命令：`set key value `
- 范例：`set name kansas`

**2. 数据查询**

- 功能：根据key查询对应的value，如果不存在，返回空 (nil）
- 命令：`get key`
- 范例：`get name`

**3. 数据删除**

- `del key`
- 删除成功返回(integer) 1
- 删除失败返回(integer) 0

<br>


#### String

**1. 基本操作**
- 添加/修改多个数据 `mset key1 value1 key2 value2 ...`
- 获取多个数据 `mget key1 key2...`(m为multi)
- 获取数据字符个数 `strlen key`
- 追加信息到原始信息后部 `append key value` （若原始信息不存在则新建
- incr/decr 对纯数字的value进行++/-- `incr key / decr key` (后面跟相反数可以达到相反效果)
- incrby / decrby 增加/减少指定数值 `incrby / decrby key num`
- incrbyfloat 一次加小数  `incrbyfloat key num`

**2. 扩展操作**
- 设置数据具有指定的生命周期 
xx秒后失效 `setex key seconds value `
xx毫秒后失效 `psetex key milliseconds value`


######**Tips**
- string在redis内部存储默认为一个字符串，当遇到增减类操作变转成数值型进行计算。
- redis所有的操作都是原子性的，采用单线程处理所有业务，命令是一个个执行的，因此无须考虑并发带来的数据影响。
- redis用于控制数据库表主键id，为数据库表主键提供生成策略，保障数据库的主键唯一性。
- redis可以用来控制数据的生命周期，通过数据是否失效控制业务行为，适用于所有具有时效性限定控制的操作。
- redis用于各种结构性和非结构性高热度数据访问加速


#### Hash

- 新的存储需求：对一系列存储的数据进行编组，方便管理，典型应用存储对象信息
- 需要的存储结构：一个存储空间保存多个键值对数据
- 相当于redis中的小型redis key - [hash:field1->value1,field2->value2...]

**1. 基本操作**
- 添加修改数据 `hset key field value`
- 获取数据
   - 单个获取`hget key field`
   - 多个获取`hgetall key`
   - 删除数据`hdel key field1 [field2]`
- 添加/修改多个数据 `hmset key field1 value1 field2 value2 ...`
- 获取多个数据`hmget key field1 field2 ...`
- 获取哈希表中字段的数量`hlen key` (有多少个field)
- 获取哈希表中是否存在指定的字段`hexists key field`

**2. 扩展操作**
- 获取哈希表中所有的字段名或字段值 `hkeys key` / `hvals key`
- 设置指定字段的数值数据增加指定范围的值
`hincrby key field increment` / `hincrbyfloat key field increment`

######**Tips**
- hash类型下的value只能存储字符串，不允许存储其他数据类型，不存在嵌套现象。
- 每个hash可以存储2^32 - 1个键值对
- hgetall可以获取全部属性，如果内部field过多，遍历整体数据效率就会很低，有可能成为数据访问瓶颈

#### list

- 数据存储要求：存储多个数据，并对数据进入存储空间的顺序进行区分
- 需要的存储结构：一个存储空间保存多个数据，且通过数据可以体现进入顺序
- list类型：保存多个数据，底层使用双向链表存储结构实现

**1. 基本操作**
- 添加修改数据 `lpush key value1 [value2] ...` / `rpush key value1 [value2]... `
- 获取数据 `lrange key start stop` (0,-1)则查看全部
- 获取索引的数据`lindex key index`
- 获取长度`llen key`
- 获取并移除数据 `lpop/rpop key` 

**2. 扩展操作**
- 移除指定数据`lrem key count value` (count 为移除多少个 因为可存在重复)
   - count > 0 从表头删到表尾
   - count < 0 从表尾删到表头
   - count = 0 表里的全删

