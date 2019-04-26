### Redis简介
Redis是一款开元的使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存也可持久化的日志型、Key-Value高性能数据库、Redis与其他Key-Value缓存产品相比有以下三个特点：
- 支持数据持久化，可以将内存中的数据保存在磁盘中，重启可再次加载使用
- 支持简单的Key-Value类型的数据，同时还提供List、Set、Zset、Hash等数据结构的存储
- 支持数据的备份，即Master-Slave模式的数据备份

Redis的优势：
- 读写速度和性能极高
- 具有丰富的数据类型
- Redis所有操作都是原子的
- 丰富的特性，如支持publish/subscribe、notify、expire等

### Redis安装、启动
Redis下载页：https://redis.io/download，下载Redis
```
wget http://download.redis.io/releases/redis-5.0.4.tar.gz
```
解压
```
tar -zxvf redis-5.0.4.tar.gz
```
进入redis目录编译到指定目录
```
cd /usr/local/redis-5.0.4
make
```
修改redis.conf文件，将```daemonize```设置为```yes```，使之后台运行  
进入src目录启动redis
```
./redis-server ../redis.conf
```
关闭redis有两种方式：
- 安全关闭```redis-cli shutdown```,这种写法只适用于没有配置密码的场景，比较不安全
- 强制关闭，```kill -9 pid```,可能造成数据未保存

### Redis登录授权
打开redis.conf, 找到```requirepass```部分，设置自己的密码即可   
重启redis，授权登录有两种方法：
- 连接的时候直接指定密码
```
redis-cli -h yourhost -p yourport -a yourpassword
```
- 连接后授权
```
redis-cli -h yourhost -p your port, auth yourpassword
```
在配置了密码的情况下，没有进行授权，连接时不会报错，但是对redis发送的命令，将返回```(error) NOAUTH Authentication required.```
### Redis配置文件redis.conf常用配置
配置|作用|默认
---|---|---
```bind```|当配置了bind之后，只有bind指定的ip可以直接访问redis，若选项为空或者为```0.0.0.0```的话，接受所有来自于可用网络接口的连接|127.0.0.1
```protected-mode```|protected-mode是Redis3.2之后的新特性，用于加强Redis的安全管理，在bind和密码均未设置的时候起作用，此时若```protected-mode=yes```,访问redis报错，只能通过本机访问|yes
```port```|访问端口|6379
```tcp-backlog```|半连接队列的大小|511
```timeout```|指定在一个client空闲多少秒后就关闭它，0就不管|0
```tcp-keepalive```|设置tcp协议的keepalive，发现死的连接，从中间网络设备的角度看连接是否存活|300
```daemonize```|指定redis是否以守护进程的方式启动|no
```supervised```|这个参数表示可以通过upstart和systemd管理redis守护进程，具体和操作系统相关，资料也不是很多|no
```pidfile```|当redis以守护进程方式运行的时候，redis默认会把pid写到pidfile指定的文件中|/var/run/redis_6379.pid
```loglevel```|指定redis的日志级别：<br/>```debug```: 非常多信息，适合开发/测试<br/>```verbose```: 很多很少有用的信息，但是没有debug的信息混乱<br/> ```notice```: 适度的verbose级别的输出，很可能是生产环境中想要的<br/>```warning```: 只记录非常重要/致命的信息|notice
```logfile```|配置log文件地址，默认打印在命令行终端的窗口上|""
```databases```|设置redis数据库的数量，默认使用0号db|16
save|redis数据保存到磁盘上，这个是RDB的时候用的|save 900 1<br/>save 300 10<br/> save 60 10000
```stop-writes-on-bgsave-error```|当启用了RDB且最后一次后台保存数据失败，redis是否停止接收数据。这会让用户意识到数据没有正确持久化到磁盘上，否则没有人会注意到灾难（disaster）的发生，如果redis重启了，那么又可以重新开始接收数据了|yes
```rdbcompression```|是否在RDB的时候使用LZF压缩字符串，如果希望省点CPU，就设为no，不过数据集就会比较大|yes
```rdbchecksum```|是否校验RDB文件，在RDB文件中有一个checksum专门用于校验|yes
```dbfilename```|dump的文件位置|dump.rdb
```dir```|redis工作目录|./
```slaveof```|主从复制，使用slaveof让一个节点成为某个节点的副本，这个只需要在副本上配置|关闭
```masterauth```|如果主机使用了requirepass配置进行密码保护，使用这个配置告诉副本连接的时候需要鉴权|关闭
```slave-serve-stale-data```|当一个slave与master失去联系或者复制正在进行中，slave可能会有两种表现：<br/>1. 如果为yes，slave仍然会应答客户端请求，但返回的数据可能是过时的或者数据可能是空的<br/>2. 如果为no，在执行除了INFO、SLAVEOF两个命令之外，都会应答```SYNC with master in progres```错误|yes
```slave-read-only```|配置redis的slave实例是否接受写操作，即slave是否为只读redis|yes
```slave-priority```|从站优先级是可以从redis的INFO命令输出中查到的一个整数。当主站不能正常工作时，redis sentinel使用它来选择一个从站并将它提升为主站。<br/>低优先级的从站被认为更适合于提升，因此如果有三个从站优先级分别是10， 100， 25，sentinel会选择优先级为10的从站，因为它的优先级最低。<br/>然而优先级值为0的从站不能执行主站的角色，因此优先级为0的从站永远不会被redis sentinel提升。|100
```requirepass```|设置客户端认证密码|关闭
```rename-command```|命令重命名，对于一些危险命令例如：```flushdb```(清空数据库)、```flushall```(清空所有记录)、```config```(客户端连接后可配置服务器)、```keys```(客户端连接后可查看所有存在的键)，作为服务端redis-server，常常需要禁用以上命令来使得服务器更加安全，警用的具体做法是：```rename-command FLUSHALL ""```，也可以保留命令但是不能轻易使用，重命名这个命令即可：```rename-command FLUSHALL abcdefg```这玩意，重启服务器后需要使用新命令来执行操作，否则服务器会报错```unknown command```|关闭
```maxclients```|设置同时连接的最大客户端数量，一旦达到了限制，redis会关闭所有的新连接并发送一个```max number of clients reached```的错误|关闭，默认10000
```maxmemory```|不要使用超过指定数量的内存，一旦达到了，redis会尝试使用驱逐策略来移除键|关闭
```maxmemory-policy```|当达到了maxmemory之后redis移除数据策略：<br/>1. ```volatile-lru```，使用LRU算法，移除范围Wie设置了失效时间的<br/>2. ```allkeys-lru```，根据LRU算法，移除范围为所有的<br/>3. ```volatile-random```，使用随机算法，移除范围为设置了失效时间的<br/>4. ```allkeys-randome```，使用随机算法，移除范围为所有的<br/>5. ```volatile-ttl```，移除最近过期的数据<br/>6. ```noeviction```，不过期，当写操作的时候返回错误<br/>注意，当写操作且redis发现没有发现有合适的数据可以移除的时候，将会报错|关闭，```noeviction```
```appendonly```|是否开启AOF|no
```appendfilename```|AOF文件名称|appendonly.aof
```appendfsync```|操作系统实际写数据到磁盘的频率：<br/>```always```，每次有写操作都进行同步，慢，但是安全<br/>```everysec```，对写操作进行累积，每秒同步一次，是一种折中方案<br/>```no```，当操作系统flush缓存的时候同步，性能更好但是会有数据丢失的风险|```everysec```
```no-appendfsync-on-rewrite```|


