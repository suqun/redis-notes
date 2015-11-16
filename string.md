##Redis String 命令##

####Set####
- set key value
- 把值value赋给key，如果key不存在，新增；否则，更新

####Setnx (set if not exists)####
- setnx key value
- 只insert不update，key不存在时设置key的值为value，返回1

####Setex####
- setex key seconds value 
- 设置key的过期时间和值

####mset####
- mset key value  [key value …] 
- 同时设置多个key-value

####msetnx####
- msetnx key value [key value …] 
- 所有key都不存在时执行set操作

####get####
- get key
- 获取key的值

####mget####
- mget key ［key …］
- 批量获取key的值。可以减少网络连接消耗

####getrange####
- getrange key start end
- 获取key中value的子串，例如 set username ‘larry’  ,getrange username 0 2 ,返回 ‘lar’

####getset####
- getset key value
- 设置key的值，并返回key的旧值. 如果key不存在，则设置后返回空

####append####
- append key value
- key存在，在旧值的后面追加value；key不存在，直接set。返回设置后的字符串长度

####setrange （替换部分子串####
- setrange key offset value
- 用value重写key值的一部分，偏移量由offset决定

####incr/decr （递增／递减）####
- incr/decr key
- key中如果存储的是数字，则可以通过incr递增key的值，返回递增后的值，如果key不存在，则视为初始值为0

####incrby／decrby ####
- incrby key increment 
- 指定步长增加key存储的数字，如果increment为负数，则减。返回操作后key的值

####del####
- del key [key …]
- 删除指定的key，返回删除key的个数

####strlen####
- strlen key     
- 返回key存储值的字符串长度