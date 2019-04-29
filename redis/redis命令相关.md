### String数据结构的基本操作
典型的Key-Value数据，根据redis官方文档，value最大值为512M。
命令|描述|用法
----|----|----
```SET```|将字符串值Value关联到Key<br/>Key已关联则覆盖，无视类型<br/>原本Key带有生存时间TTL，那么TTL被清除|```SET key value [EX seconds] [PX milliseconds] [NX|XX]```
```GET```|返回Key关联的字符串值<br/>Key不存在返回nil<br/>Key存储的不是字符串，返回错误，因为GET只用于处理字符串|```GET key```
```MSET```|同时设置一个或多个Key-Value键值对<br/>某个给定的Key已经存在，那么MSET新值会覆盖旧值<br/>如果不希望被覆盖，那么使用```MSETNX```命令<br/>MSET是一个原子性操作，所有Key都会在同一时间被设置，不会存在有些更新有些没更新的情况|```MSET key value [key value ...]```
```MGET```|返回一个或多个给定Key对应的Value<br/>某个Key不存在那么这个Key就返回nil|```MGET key [key ...]```
```SETEX```|将Value关联到Key<br/>设置Key生存时间为seconds，单位秒<br/>如果Key对应的Value已经存在，则覆盖旧值<br/>SET也可以设置失效时间，但是不同在于SETEX是一个原子操作，即关联值与设置生存时间同一时间完成|```SETEX key seconds value```
```SETNX```|将Key的值设置为Value，当且仅当Key不存在<br/>若给定的Key已经存在，SETNX不做任何动作|```SETNX key value```
```INCR```|key中存储的数字值+1，返回增加之后的值<br/>key不存在，那么key的值被初始化为0再执行INCR<br/>如果值包含错误类型或者字符串不能被表示为数字，那么返回错误<br/>值限制在64位有符号数字表示之内，即-9223372036854775808~9223372036854775807|```INCR key```
```DECR```|key中存储的数字值-1，其余同INCR|```DECR key```
```INCRBY```|将key中所存储的值加上增量返回增加之后的值，其余同INCR|```INCRBY key increment```
```DECRBY```|将key所存储的值减去减量decrement，其余同INCR|```DECRBY key decrement```

### Hash数据结构相关操作
命令|描述|用法
---|---|---
```HSET```|将哈希表key中的域field的值设为value<br/>key不存在，一个新的hash表被创建<br/>field已经存在，旧的值被覆盖|```HSET key field value```
```HGET```|返回哈希表key中给定域field的值|```HGET key field```
```HDEL```|删除哈希表key中的一个或多个指定域，不存在的域将被忽略|```HDEL key filed [field ...]```
```HEXISTS```|查看哈希表key中，给定域field是否存在，存在返回1，不存在返回0|```HEXISTS key field```
```HGETALL```|返回哈希表key中，所有的域和值|```HGETALL key```
```HINCRBY```|为哈希表key中的域field加上增量increment，其余同INCR命令|```HINCRYBY key filed increment```
```HKEYS```|返回哈希表key中的所有域|```HKEYS key```
```HLEN```|返回哈希表key中域的数量|```HLEN key```
```HMGET```|返回哈希表key中，一个或多个给定域的值<br/>如果给定的域不存在，那么返回一个nil值|```HMGET key field [field ...]```
```HMSET```|同时将多个field-value对设置到哈希表key中<br/>会覆盖哈希表中已存在的域<br/>key不存在，那么一个空哈希表会被创建并执行HMSET操作|```HMSET key field value [field value ...]```
```HVALS```|返回哈希表key中所有的域和值|```HVALS key```

### List数据相关操作
命令|描述|用法
```LPUSH```|将一个或多个value插入到列表key的表头<br/>如果有多个value，那么各个value值按从左到右的顺序依次插入表头<br/> key不存在，，一个空列表会被创建并执行LPUSH操作<br/>key存在但不是列表类型，返回错误|```LPUSH key value [value ...]```
```LPUSHX```|将value插入到key的表头，当且仅当key存在且为一个列表<br/>key不存在时，LPUSHX命令什么都不做|```LPUSHX key value```
```LPOP```|移除并返回列表key的头元素|```LPOP key```
```LRANGE```|返回列表中指定区间内的元素，区间以偏移量start和stop指定<br/>start和stop都以0为底<br/>可使用负数下标，-1表示最后一个元素，-2表示倒数第二个元素……以此类推<br/>start 大于列表最大小标，返回空列表<br/>stop大于列表最大下标，stop=列表最大下标|```LRANGE key start stop```
```LREM```|根据count的值，移除列表中与value相等的元素<br/>count>0，表示从头到尾搜索，移除与value相等的元素，数量为count<br/>count<0，表示从尾到头搜索，移除与value相等的元素，数量为count<br/>count=0，表示移除表中所有与value相等的元素|```LREM key count value```
```LSET```|将列表key下标为index的元素值设为value<br/>index参数超出范围，或对一个空列表进行LSET时，返回错误|```LSET key index value```
```LINDEX```|返回列表key中，下标为index的元素|```LINDEX key index```
```LINSERT```|将value插入列表key中，位于pivot前面或者后面<br/>pivot不存在与列表key时，不执行任何操作<br/>key不存在，不执行任何操作|```LINSERT key BEFORE|AFTER pivot value```
```LLEN```|返回列表key的长度<br/>列表不存在，返回0|```LLEN key```
```LTRIM```|对一个列表进行修剪，让列表只返回指定区间内的元素|```LTRIM key start stop```
```RPOP```|移除并返回列表key的尾元素|```RPOP key```
```RPOPLPUSH```|在一个原子事件内，执行两个动作：<br/>将列表source中最后一个元素弹出并返回给客户端<br/>将source弹出的元素插入到列表destination,作为destination列表的头元素|```RPOPLPUSH source destination```
```RPUSH```|将一个或多个value插入到列表key的表尾|```RPUSH key value [value ...]```
```RPUSHX```|将value插入到列表key的表尾，当且仅当key存在并且是一个列表<br/>key不存在，RPUSHX什么都不做|```RPUSHX key value```

###Set数据结构相关操作
```SADD```|将一个或多个member元素加入到key中，已存在在集合的member将被忽略<br/>假如key不存在，则只创建一个只包含member元素做成员的集合<br/>当key不是集合类型时，将返回错误|```SADD key number [member ...] ```
```SCARD```|返回key对应的集合中的元素数量|```SCARD key```
```SDIFF```|返回一个集合的全部成员，该集合是第一个key对应的集合和后面key对应的集合的差集|```SDIFF key [key ...]```
```SDIFFSTORE```|和SDIFF类似，但结果保存到destination集合而不是简单返回结果集<br/>destination已存在，则覆盖|```SDIFFSTORE destionation key [key ...] ```
```SINTER```|返回一个集合的全部成员，该集合是所有给定集合的交集<br/>不存在的key被视为空集|```SINTER key [key ...] ```
```SINTERSTORE```|和SINTER类似，但结果保存到destination集合而不是简单返回结果集<br/>如果destination已存在，则覆盖<br/>destination可以是key本身|```SINTERSTORE destination key [key ...] ```
```SISMEMBER```|判断member元素是否key的成员，0表示不是，1表示是|```SISMEMBER key member ```
```SMEMBERS```|返回集合key中的所有成员，不存在的key被视为空集|```SMEMBERS key```
```SMOVE```|原子性地将member元素从source集合移动到destination集合<br/>source集合中不包含member元素，SMOVE命令不执行任何操作，仅返回0<br/>destination中已包含member元素，SMOVE命令指示简单做source集合的member元素移除|```SMOVE source desination member```
```SPOP```|移除并返回集合中的一个随机元素，如果count不指定那么随机返回一个随机元素<br/>count为正数且小于集合元素数量，那么返回一个count个元素的数组且数组中的元素各不相同<br/>count为正数且大于等于集合元素数量，那么返回整个集合<br/>count为负数那么命令返回一个数组，数组中的元素可能重复多次，数量为count的绝对值|```SPOP key [count]```
```SRANDMEMBER```|如果count不指定，那么返回集合中的一个随机元素<br/>count同上|```SRANDMEMBER key [count]```
```SREM```|移除集合key中的一个或多个member元素，不存在的member将被忽略|```SREM key member [member ...]```
```SUNION```|返回一个集合的全部成员，该集合是所有给定集合的并集<br/>不存在的key被视为空集|```SUNION key [key ...]```
```SUNIONSTORE```|类似SUNION，但结果保存到destination集合而不是简单返回结果集<br/>destination已存在，覆盖旧值<br/>destination可以是key本身|```SUNIONSTORE destination key [key...]```