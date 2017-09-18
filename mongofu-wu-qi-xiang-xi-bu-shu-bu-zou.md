

mongo &gt;=3.2

# 历史数据服务\(27017\)配置文件

```
# mongod.conf

#where to log
logpath=/mnt/project/db_hisdata/log/mongod.log

logappend=true

# fork and run in background
fork=true

port=27017

dbpath=/mnt/project/db_hisdata

# location of pidfile
pidfilepath=/var/run/mongodb/mongod37017.pid

# Listen to local interface only. Comment out to listen on all interfaces. 
#bind_ip=192.168.0.31

# Disables write-ahead journaling
nojournal=true

# Enables periodic logging of CPU utilization and I/O wait
#cpu=true

# Turn on/off security.  Off is currently the default
noauth=false
#auth=true


storageEngine=wiredTiger
wiredTigerCacheSizeGB=4
wiredTigerStatisticsLogDelaySecs=0
wiredTigerJournalCompressor=zlib
wiredTigerDirectoryForIndexes=true
wiredTigerCollectionBlockCompressor=snappy
wiredTigerIndexPrefixCompression=true
directoryperdb=true



```

# 配置数据服务器\(27018\)配置文件

```
# mongod.conf

#where to log
logpath=/mnt/project/db_configure/log/mongod.log

logappend=true

# fork and run in background
fork=true

port=27018

dbpath=/mnt/project/db_configure

# location of pidfile
pidfilepath=/var/run/mongodb/mongod37018.pid

# Listen to local interface only. Comment out to listen on all interfaces. 
#bind_ip=192.168.0.31

# Disables write-ahead journaling
nojournal=true

# Enables periodic logging of CPU utilization and I/O wait
#cpu=true

# Turn on/off security.  Off is currently the default
noauth=false
#auth=true


storageEngine=wiredTiger
wiredTigerCacheSizeGB=2
wiredTigerStatisticsLogDelaySecs=0
wiredTigerJournalCompressor=zlib
wiredTigerDirectoryForIndexes=true
wiredTigerCollectionBlockCompressor=snappy
wiredTigerIndexPrefixCompression=true
directoryperdb=true


```

# 

# 启动

，用mongo命令行设置用户名及密码

```
 #mongod --config /mnt/project/db_hisdata/mongod.conf
 #mongod --config /mnt/project/db_configure/mongod.conf
```

# 用户名密码设置

先用无密码模式启动mongod（对应conf文件中 noauth=true）



```

#mongo 127.0.0.1
#use admin
#db.createUser({})
#use beopdata
#db.createUser({})
```

# 要在云后台开放防火墙端口\(27017,27018\)

远程用mongochef验证是否连接成功。



