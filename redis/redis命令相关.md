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