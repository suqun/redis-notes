##Redis 的安装与配置

###安装###
1. download redis 3.0.5

2.
  
  tar zxvf redis-3.0.5.tar.gz

  cd redis-3.0.5/src
  
  make install
  
  

3. 测试

  redis-server &

###配置###

  
  mkdir -p /usr/local/redis/bin
  
  mkdir -p /usr/local/redis/tec

  cp redis-cli redis-check-aof redis-check-dump redis-benchmark redis-server redis-sentinel redis-trib.rb mkreleasehdr.sh /usr/local/redis/bin

  cp redis.conf sentinel.conf /usr/local/redis/etc
  
  
###启动

 5. start server
 
  1) 使用默认配置文件启动
     
    
     redis-server &
  
     ps -ef|grep redis
     
     netstat-tunpl|grep 6379
     
     redis-cli(shutdown)
   
     
  2) 指定配置文件启动
      
       redis-server /usr/local/redis/etc/redis.conf
  
       vim redis.conf
       
            daemonize no --> daemonize yes
       
  3) redis_init_script Linux开机启动配置
  
  	 cd /opt/redis-3.0.5/utils

  	 
     1 `vim redis_init_script`
     
       EXEC
       
       CLIEXEC
       
     2 

       mkdir -p /etc/redis
     
       cp /opt/redis-3.0.5/redis.conf /etc/redis/6379.conf
       
       vim 6379.conf
       
          daemonize no --> daemonize yes
          
       cp redis_init_script /etc/init.d/redisd

       sudo update-rc.d redisd defaults
       
       service redisd start
       
 ###停止

 6. shutdown
  
  	   Saving the final RDB snapshot before exiting.
       10867:M 07 Nov 13:40:32.603 # Failed opening .rdb for saving: Permission denied
          cd /etc/redis
          vim 6379.conf
          如果是./表示：运行reides命令的目录(例如，$redis_home中，执行./bin/redis-server etc/redis.conf命令，./则代表的是$redis_home)。
          查看当前用户对dir所配置的路径是否有写权限。


  kill -9 PID


###设置环境变量###
7. cd ~

   vim .bashrc
   
       #redis
       
       export REDIS_HOME=/usr/local/redis
       
       export PATH=$REDIS_HOME/bin:$PATH
    
   source .bashrc


