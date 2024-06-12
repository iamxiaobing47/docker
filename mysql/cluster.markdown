```shell

[mysqld]
##设置server_id,同一局域网中需要唯一
server_id=101
##指定不需要同步的数据库名称
binlog-ignore-db=mysql
##开启二进制日志功能
log-bin=mall-mysql-bin
##设置二进制日志使用内存大小（事务）
binlog_cache_size=1M
##设置使用的二进制日志格式（mixed,statement,row）
binlog_format=mixed
##二进制日志过期清理时间。默认值为0，表示不自动清理。
expire_logs_days=7
##跳过主从复制中遇到的所有错误或指定类型的错误，避免slave端复制中断。
##如：1062错误是指一些主键重复，1032错误是因为主从数据库数据不一致
slave_skip_errors=1062
```
```shell
CREATE USER 'slave'@'%' IDENTIFIED BY '123456';
```
```shell
GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'slave'@'%';
```

```shell
[mysqld]
server_id=102
binlog-ignore-db=mysql
log-bin=mall-mysql-slave1-bin
binlog_cache_size=1M
binlog_format=mixed
expire_logs_days=7
slave_skip_errors=1062


## relay_log
配置中继日志
relay_log=mall-mysql-relay-bin
## log_slave_updates表示slave将复制事件写进自己的二进制日志
log_slave_updates=1
## slave设置为只读（具有super权限的用户除外）
read_only=1
```

```shell
change master to master_host='宿主机ip',
master_user='slave',
master_password='123456', master_port=3307,
master_log_file='mall-mysql-bin.000001',
master_log_pos=617, master_connect_retry=30;
```