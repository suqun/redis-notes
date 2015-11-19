##Redis ZSet 有序集合命令

####Zadd
- zadd key score member
- 用来向有序集合中添加一个元素和该元素的分数，返回值表示成功加入的元素数量（不包含原来已经存在的元素）。可一次添加多个元素及其元素的分数
```
127.0.0.1:6379> zadd zstudents 10 tom 20 alice 18 larry
(integer) 3
127.0.0.1:6379> zadd zstudents 19 larry
(integer) 0
```

####Zrem
- zrem key member 
- 用来删除一个或多个元素，返回值表示成功删除的元素数量
```
127.0.0.1:6379> zrem zstudents tom
(integer) 1
```

####Zcard
- zcard key
- 用来获得集合中元素的数量
```
127.0.0.1:6379> zcard zstudents
(integer) 2
```

####Zscore
- zscore key member
- 用来获得元素的分数
```
127.0.0.1:6379> zscore zstudents larry
"19"
```

####Zincrby
- zincrby key increment member
- 用来增加一个元素的分数，返回值是更改后的分数
```
127.0.0.1:6379> zincrby zstudents 1 larry
"20"
127.0.0.1:6379> zscore zstudents larry
"20"
127.0.0.1:6379> zincrby zstudents -2 larry
"18"
```

####ZRangeByScore
- zrangebyscore key min max [withscores] [limit offset count]
- 按照分数从小到大的顺序返回分数在 min 和 max （包含 min 和 max）之间的元素
  1. 若不包含端点，可在分数前加上“(”
  2. Min 和 max 支持无穷大 inf
  3. Withscore 返回的数据格式为 元素+分数
  4. LIMIT 表示在获得元素的基础上从第 offset 个开始的 count 个元素
```
127.0.0.1:6379> zadd zstudents 21 lily 35 chely 29 jim 50 rose
(integer) 4
127.0.0.1:6379> zrangebyscore zstudents 20 30
1) "alice"
2) "lily"
3) "jim"
127.0.0.1:6379> zrangebyscore zstudents 20 30 withscores
1) "alice"
2) "20"
3) "lily"
4) "21"
5) "jim"
6) "29"
127.0.0.1:6379> zrangebyscore zstudents 20 30 withscores limit 0 2
1) "alice"
2) "20"
3) "lily"
4) "21"
127.0.0.1:6379> zrangebyscore zstudents 20 30 withscores limit 1 2
1) "lily"
2) "21"
3) "jim"
4) "29"
127.0.0.1:6379> zrangebyscore zstudents 20 +inf withscores
 1) "alice"
 2) "20"
 3) "lily"
 4) "21"
 5) "jim"
 6) "29"
 7) "chely"
 8) "35"
 9) "rose"
 127.0.0.1:6379> zrangebyscore zstudents (20 +inf withscores
1) "lily"
2) "21"
3) "jim"
4) "29"
5) "chely"
6) "35"
7) "rose"
8) "50"
```

####Zcount
- zcount key min max
- 用来获得指定分数范围内的元素个数
```
127.0.0.1:6379> zcount zstudents (20 +inf 
(integer) 4
```

####ZRemRangeByScore
- zremrangebyscore key min max
- 按照分数范围删除元素，返回值是删除的元素数量
```
127.0.0.1:6379> zremrangebyscore zstudents 50 +inf
(integer) 1
```

####Zrange(ZRevRange)
- zrange (zrevrange) key start stop [withscores]
- 按照元素分数从小（大）到大（小）的顺序返回索引从 start 到 stop 之间的所有元素
```
127.0.0.1:6379> zcard zstudents 
(integer) 5
127.0.0.1:6379> zrange zstudents 0 5
1) "larry"
2) "alice"
3) "lily"
4) "jim"
5) "chely"
127.0.0.1:6379> zrange zstudents 0 -1 withscores
 1) "larry"
 2) "18"
 3) "alice"
 4) "20"
 5) "lily"
 6) "21"
 7) "jim"
 8) "29"
 9) "chely"
10) "35"
127.0.0.1:6379> zrevrange zstudents 0 -1 withscores
 1) "chely"
 2) "35"
 3) "jim"
 4) "29"
 5) "lily"
 6) "21"
 7) "alice"
 8) "20"
 9) "larry"
10) "18"
```

####ZRank (ZRevRank)
- zrank (zrevrank) key member
- 用来获得元素的排名，分数最小（大）的排名为0
```
127.0.0.1:6379> zrange zstudents 0 -1 withscores
 1) "larry"
 2) "18"
 3) "alice"
 4) "20"
 5) "lily"
 6) "21"
 7) "jim"
 8) "29"
 9) "chely"
10) "35"
127.0.0.1:6379> zrank zstudents alice
(integer) 1
127.0.0.1:6379> zrank zstudents chely
(integer) 4
```

####ZRemRangeByRank
- zremrangebyrank key start stop
- 此命令用来删除指定排名范围内的所有元素，并返回删除的元素数量
```
127.0.0.1:6379> zremrangebyrank zstudents 0 1
(integer) 2
127.0.0.1:6379> zrange zstudents 0 -1 withscores
1) "lily"
2) "21"
3) "jim"
4) "29"
5) "chely"
6) "35"
```

####ZInterStore
- zinterstore dest numkeys key [key ...] [weights weight] [aggregate sum/min/max]
- 此命令用来计算多个有序集合的交集并将结果存储在 dest 键中，返回值为 dest 键中的元素个数
    1. WEIGHTS 参数设置每个集合的权重，运算时，内部元素分数乘以该集合的权重
    2. AGGREGATE 参数用来决定 dest 键中元素的分数，可求和、最大值、最小值
```
127.0.0.1:6379> zadd chinese 90 tom 88 lily 98 alice
(integer) 3
127.0.0.1:6379> zadd math 85 tom 80 lily 96 alice
(integer) 3
127.0.0.1:6379> zadd english 70 tom 92 lily 90 alice
(integer) 3
127.0.0.1:6379> zinterstore totalscore 3 chinese math english  //默认sum
(integer) 3
127.0.0.1:6379> zrange totalscore 0 -1 withscores
1) "tom"
2) "245"
3) "lily"
4) "260"
5) "alice"
6) "284"
127.0.0.1:6379> zinterstore minscore 3 chinese math english aggregate min
(integer) 3
127.0.0.1:6379> zrange minscore 0 -1 withscores
1) "tom"
2) "70"
3) "lily"
4) "80"
5) "alice"
6) "90"
127.0.0.1:6379> zinterstore maxscore 3 chinese math english aggregate max
(integer) 3
127.0.0.1:6379> zrange maxscore 0 -1 withscores
1) "tom"
2) "90"
3) "lily"
4) "92"
5) "alice"
6) "98"

127.0.0.1:6379> zinterstore totalscore2 3 chinese math english weights 0.4 0.3 0.3
(integer) 3
127.0.0.1:6379> zrange totalscore2 0 -1 withscores
1) "tom"
2) "82.5"
3) "lily"
4) "86.799999999999997"
5) "alice"
6) "95"
```
