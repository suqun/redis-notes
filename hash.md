##Redis Hash笔记

####hset
- hset key field value  
- 设置hash表key中的field的值。如果hash表不存在则创建，并执行设置field的值，如果hash表存在，field的值覆盖或更新

####hmset
- hmset key field value [field value  …]
- 批量设置hash表key的域

####hsetnx
- hsetnx key field value
- 仅仅当field域不存在时，设置hash表的feild域

####hget
- hget key field
- 获取hash表key的field值

####hmget
- hmget key field [field …]
- 批量获取hash表key中的field值

####hgetall
- hgetall key 
- 获取hash表key中的所有field及其值

####hkeys
- hkeys key
- 获取hash表所有的key

####hvals
- hvals key
- 获取hash表中key的所有field值

####hexists
- hexists key field
- 判断hash表中key的某个field是否存在，存在返回1，不存在返回0

####hincrby                                                          
- hincrby key field increment
- hash表field域的数值增加步长increment，如果increment为负，则递减。如果field域不存在，初始值视为0。返回递增后的值

####hdel                                          hash删除及其他命令
- hdel key field [field …]
- 删除hash表中的field，指定多个field，则删除多个

####hlen
- hlen key
- 获取hash表中key的域数