###Redis笔记  
  <br>
   

Redis(Remote Dictionary Server)是用C语言开发的一个开源的高性能键值对(key-value)数据库。

---
####Redis特点
- 数据间没有必然的联系
- 内部采用单线程机制进行工作ß
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

####基本类型操作

**1. String**
- 添加/修改多个数据 `mset key1 value1 key2 value2 ...`
- 获取多个数据 `mget key1 key2...`(m为multi)
- 获取数据字符个数 `strlen key`
- 追加信息到原始信息后部 `append key value` （若原始信息不存在则新建

