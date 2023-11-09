# redis命令


[toc]
# 数据类型
### Redis键(key)
- 查看当前库所有key    (匹配：keys *1)
	```
	keys *
	```
- 判断某个key是否存在
	```
	exists key
	```
- 查看你的key是什么类型
	```
	type key 
	```
- 删除指定的key数据
	```
	del key
	```
- 根据value选择非阻塞删除
	> 仅将keys从keyspace元数据中删除，真正的删除会在后续异步操作。
	```
	unlink key
	```
- 10秒钟：为给定的key设置过期时间
	```
	expire key 10
	```
- 查看还有多少秒过期，-1表示永不过期，-2表示已过期
	```
	ttl key 
	```
- 命令切换数据库
	```
	select
	```
- 查看当前数据库的key的数量
	```
	dbsize
	```
- 清空当前库
	```
	flushdb
	```
- 通杀全部库
	```
	flushall
	```
### Redis字符串(String)
##### 简介
- 最基本的数据类型，一个key对应一个value，一个Redis中字符串value最多可以是512M
- 二进制安全的。意味着Redis的string可以包含任何数据。比如jpg图片或者序列化的对象。
##### 常用命令
- 添加键值对
	```
	set <key> <value> [EX seconds|PX miiliseconds|KEEPTTL] [NX|XX]
	```
	> NX：当数据库中key不存在时，可以将key-value添加数据库
	> XX：当数据库中key存在时，可以将key-value添加数据库，与NX参数互斥
	> EX：key的超时秒数
	> PX：key的超时毫秒数，与EX互斥

- 查询对应键值
	```
	get <key>
	```
- 将给定的value 追加到原值的末尾
	```
	append <key> <value>
	```
- 获得值的长度
	```
	strlen <key>
	```
- 只有在 key 不存在时，设置 key 的值
	```
	setnx <key> <value>
	```
- 将 key 中储存的数字值增1
	> 只能对数字值操作，如果为空，新增值为1
	```
	incr <key>
	```
- 将 key 中储存的数字值减1
	> 只能对数字值操作，如果为空，新增值为-1
	```
	decr <key>
	```
- 将 key 中储存的数字值增减。自定义步长。
	```
	incrby / decrby  <key><步长>
	```
# 特性
### 原子性
> 原子操作是指不会被线程调度机制打断的操作；
这种操作一旦开始，就一直运行到结束，中间不会有任何 context switch （切换到另一个线程）。
- 在单线程中， 能够在单条指令中完成的操作都可以认为是"原子操作"，因为中断只能发生于指令之间。
- 在多线程中，不能被其它进程（线程）打断的操作就叫原子操作。
- Redis单命令的原子性主要得益于Redis的单线程。
