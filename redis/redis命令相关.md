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