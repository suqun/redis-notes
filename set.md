##Redis Set无序集合命令

####Sadd
- Sadd key member
- 用来向集合中添加一个或多个元素，返回成功添加的元素个数。因为集合中不存在相同的元素，若加入的元素已经存在于集合中则忽略

####Srem
- Srem key member
- 用来从集合中删除一个或多个元素，返回值表示多个成功删除的数量。

####Spop
- Spop key
- 从集合中随机选择一个元素弹出，返回弹出的元素值

####Smembers
- Smembers key
- 获取集合中所有的元素
```
127.0.0.1:6379> smembers students
1) "2"
2) "3"
3) "4"
```

####SRandMember
- Srandmember key ［count］
- 随机从集合中取一个或多个元素
    1. count>0时，返回count个不重复的元素
    2. count<0时，返回|count|个元素，这些元素可能相同
```
127.0.0.1:6379> srandmember students
"2"
127.0.0.1:6379> srandmember students 2
1) "4"
2) "2"
127.0.0.1:6379> srandmember students 2
1) "4"
2) "3"
127.0.0.1:6379> srandmember students -2
1) "3"
2) "4"
127.0.0.1:6379> srandmember students -2
1) "4"
2) "3"
127.0.0.1:6379> srandmember students -2
1) "3"
2) "3"
```

####SIsMember
- SIsMember key member
- 判断元素member是否在集合中，存在返回1，不存在返回0
```
127.0.0.1:6379> sismember students 5
(integer) 0
127.0.0.1:6379> sismember students 2
(integer) 1
```

####Scard
- Scard key
- 获取集合中元素的个数
```
127.0.0.1:6379> scard students
(integer) 3
```

####Sdiff
- sdiff key [key ...]
- 对多个集合执行差集运算

![A-B](https://github.com/suqun/redis-notes/blob/master/etc/A-B.png) 
```
127.0.0.1:6379> smembers students2
1) "3"
2) "6"
127.0.0.1:6379> smembers students
1) "2"
2) "3"
3) "4"
127.0.0.1:6379> sdiff students2 students
1) "6"
127.0.0.1:6379> sdiff students students2
1) "2"
2) "4"
```

####Sinter
- Sinter key [key ...]
- 用来对多个集合执行交集运算

![A∩B](https://github.com/suqun/redis-notes/blob/master/etc/A∩B.png) 

```
127.0.0.1:6379> smembers students
1) "2"
2) "3"
3) "4"
127.0.0.1:6379> smembers students2
1) "3"
2) "6"
127.0.0.1:6379> sinter students students2
1) "3"
127.0.0.1:6379> 
```

####Sunion
- Sunion key [key ...]
- 多个集合执行并集计算

![A∪B](https://github.com/suqun/redis-notes/blob/master/etc/A∪B.png)
```
127.0.0.1:6379> smembers students 
1) "2"
2) "3"
3) "4"
127.0.0.1:6379> smembers students2
1) "3"
2) "6"
127.0.0.1:6379> sunion students students2
1) "2"
2) "3"
3) "4"
4) "6"
127.0.0.1:6379> 
```

####SDiffStore
- sdiffstore dest key [key ...]
- 此命令功能同sdiff，区别是sDiffStore不会直接返回结果，而是将结果存储在dest键中

```
127.0.0.1:6379> smembers students
1) "2"
2) "3"
3) "4"
127.0.0.1:6379> smembers students2
1) "3"
2) "6"
127.0.0.1:6379> sdiffstore studiff students students2
(integer) 2
127.0.0.1:6379> smembers studiff
1) "2"
2) "4"
```

####SInterStore
- sinterstore dest key [key ...]
- 同上

####SUnionStore
- sunionstore dest key [key ...]
- 同上 























