##Redis 键值相关命令

Keys、exists 和 del 命令

Expire、move 和 persist 命令

Randomkey、rename 和 type 命令

####Keys
- keys pattern
- 获取符合规则的键名列表。Pattern 支持 glob 风格通配符格式，规则如下
    1. `? 匹配一个字符`
    2. `* 匹配任意个字符`
    3. `[ ] 匹配括号间的任一字符，可使用-符合表示一个范围，如 a[b-d]`
    4. `\x  匹配字符x，用于转义`
    
- Keys 命令需要遍历所有键，若键的数量较多时影响性能，不建议使用
```
127.0.0.1:6379> keys student*
1) "students2"
2) "students"
3) "student"
127.0.0.1:6379> keys student?
1) "students"
```

####exists
- exists key
- 判断一个键是否存在，存在返回 1，否则返回 0。
```
127.0.0.1:6379> exists students
(integer) 1
127.0.0.1:6379> exists ssss
(integer) 0
```

####del
- del key [key ...]
- 用来删除一个或多个键，返回值表示删除的键个数。
- del 命令的参数不支持通配符，可结合 Linux的管道和xargs命令自己实现删除所有符合规则的键
```
127.0.0.1:6379> del stu:1
(integer) 1
127.0.0.1:6379> exists stu:1
(integer) 0
127.0.0.1:6379> keys stu:*
1) "stu:4:username"
2) "stu:3:username"
3) "stu:1:age"
4) "stu:2:username"
5) "stu:2"
6) "stu:1:username"
127.0.0.1:6379> quit
larrydeMacBook-Pro:~ larry$ redis-cli keys "stu:*" | xargs redis-cli del
(integer) 6
larrydeMacBook-Pro:~ larry$ redis-cli
127.0.0.1:6379> keys stu:*
(empty list or set)
```

####expire
- expire key seconds
- 用来设置一个键的生存时间，返回 1 表示设置成功，0 表示键不存在或失败。
```
127.0.0.1:6379> set stu:1:name larry
OK
127.0.0.1:6379> expire stu:1:name 10
(integer) 1
127.0.0.1:6379> exists stu:1:name
(integer) 1
127.0.0.1:6379> exists stu:1:name
(integer) 0
```

####persist
- persist key 
- 用来取消键的生存时间的设置，即将键恢复成永久的
```
127.0.0.1:6379> set stuname larry
OK
127.0.0.1:6379> expire stuname 20
(integer) 1
127.0.0.1:6379> ttl stuname
(integer) 17
127.0.0.1:6379> persist stuname
(integer) 1
127.0.0.1:6379> ttl stuname
(integer) -1
127.0.0.1:6379> get stuname
"larry"
```

####move
- move key db
- 将当前数据库中指定的键 Key 移动到参数中指定的数据库中。如果该 Key 在目标数据库中已经存在，或者在当前数据库中并不存在，该命令将不做任何操作并返回0
```
127.0.0.1:6379> move stuname 1
(integer) 1
127.0.0.1:6379> select 1
OK
127.0.0.1:6379[1]> keys *
1) "stuname"
127.0.0.1:6379[1]> select 0
OK
127.0.0.1:6379> exists stuname
(integer) 0
```

####RandomKey
- randomkey 
- 从当前打开的数据库中随机的返回一个Key，返回的随机键，如果该数据库是空的则返回 nil
```
127.0.0.1:6379> randomkey
"maxscore"
127.0.0.1:6379> randomkey
"minscore"
127.0.0.1:6379> randomkey
"zstudents"
```

####rename
- rename key newKey
- 为指定的键重新命名
```
127.0.0.1:6379> set stuname larry
OK
127.0.0.1:6379> rename stuname stu:name
OK
127.0.0.1:6379> get stu:name
"larry"
```

####type
- type key
- 用来获得键值的数据类型，返回值可能是 string、hash、list、set、zset
```
127.0.0.1:6379> type stu:name
string
127.0.0.1:6379> type students
set
```
