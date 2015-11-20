##Redis 服务器相关命令
ping、echo 与 quit 命令

select、dbsize 与 info 命令

Config  get、flushdb 和 flushall命令

####ping
- ping 
- 用来测试连接是否存活
```
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> ping
Could not connect to Redis at 127.0.0.1:6379: Connection refused
```

####echo
- echo 内容
- 用来输出一些内容
```
127.0.0.1:6379> echo larry
"larry"
```

####quit
- quit
- 退出连接
```
127.0.0.1:6379> quit
larrydeMacBook-Pro:~ larry$ 
```

####select
- select  db
- 用来选择数据库，db的值为 0 到 15。
```
127.0.0.1:6379> select 1
OK
127.0.0.1:6379[1]> dbsize
(integer) 1
```

####dbsize
- dbsize
- 返回当前数据库中键的数目。
```
127.0.0.1:6379[1]> select 0
OK
127.0.0.1:6379> dbsize
(integer) 16
```

####info
- info
- 获取服务器的信息和统计。
```
127.0.0.1:6379> info
# Server
redis_version:3.0.5
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:5da256bc46d8f9ed
redis_mode:standalone
os:Darwin 15.0.0 x86_64
arch_bits:64
multiplexing_api:kqueue
gcc_version:4.2.1
process_id:696
run_id:dcd26171d485bad495482f4ba1d1617a34ad0d4e
tcp_port:6379
uptime_in_seconds:304
uptime_in_days:0
hz:10
lru_clock:5187759
config_file:/Users/larry/Library/redis/etc/redis.conf

# Clients
connected_clients:1
client_longest_output_list:0
client_biggest_input_buf:0
blocked_clients:0

# Memory
used_memory:1011472
used_memory_human:987.77K
used_memory_rss:1642496
used_memory_peak:1011472
used_memory_peak_human:987.77K
used_memory_lua:36864
mem_fragmentation_ratio:1.62
mem_allocator:libc

# Persistence
loading:0
rdb_changes_since_last_save:0
rdb_bgsave_in_progress:0
rdb_last_save_time:1448028031
rdb_last_bgsave_status:ok
rdb_last_bgsave_time_sec:-1
rdb_current_bgsave_time_sec:-1
aof_enabled:0
aof_rewrite_in_progress:0
aof_rewrite_scheduled:0
aof_last_rewrite_time_sec:-1
aof_current_rewrite_time_sec:-1
aof_last_bgrewrite_status:ok
aof_last_write_status:ok

# Stats
total_connections_received:2
total_commands_processed:5
instantaneous_ops_per_sec:0
total_net_input_bytes:117
total_net_output_bytes:30
instantaneous_input_kbps:0.00
instantaneous_output_kbps:0.00
rejected_connections:0
sync_full:0
sync_partial_ok:0
sync_partial_err:0
expired_keys:0
evicted_keys:0
keyspace_hits:0
keyspace_misses:0
pubsub_channels:0
pubsub_patterns:0
latest_fork_usec:0
migrate_cached_sockets:0

# Replication
role:master
connected_slaves:0
master_repl_offset:0
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0

# CPU
used_cpu_sys:0.15
used_cpu_user:0.08
used_cpu_sys_children:0.00
used_cpu_user_children:0.00

# Cluster
cluster_enabled:0

# Keyspace
db0:keys=16,expires=0,avg_ttl=0
db1:keys=1,expires=0,avg_ttl=0
```

####Config get
- config get  parameter 
- 主要用于读取服务器的运行时参数。但是并不是所有的配置参数都可以通过该命令进行读取。最后需要指出的是，和 redis.conf 中不同的是，在命令中不能使用数量缩写格式，如GB、KB等，只能使用表示字节数量的整数值
```
127.0.0.1:6379> config get dir
1) "dir"
2) "/Users/larry"
127.0.0.1:6379> config get *
  1) "dbfilename"
  2) "dump.rdb"
  3) "requirepass"
  4) ""
  ...
```

####flushdb
- flushdb
- 删除当前选择数据库中的所有 key。
```
127.0.0.1:6379> select 1
OK
127.0.0.1:6379[1]> keys *
1) "stuname"
127.0.0.1:6379[1]> flushdb
OK
127.0.0.1:6379[1]> keys *
(empty list or set)
```

####flushall
- flushall
- 删除所有数据库中的所有 key
```
127.0.0.1:6379[1]> flushall
OK
127.0.0.1:6379[1]> select 0
OK
127.0.0.1:6379> dbsize
(integer) 0
```