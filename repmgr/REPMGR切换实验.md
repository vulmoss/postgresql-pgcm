# REPMGR切换实验
## repmgr命令如下：
` repmgr primary register 安装pg的repmgr扩展并注册为主节点`
` repmgr primary unregister 注销不活动的主节点`
 `repmgr standby clone 从其他节点复制数据到从节点 `
 `repmgr standby register 注册从节点（添加从的信息到repmgr元数据）``repmgr standby unregister repmgr元数据中移除从的信息``repmgr standby promote 将从提升为主``repmgr standby follow 将从跟随新主``repmgr standby switchover 将从提升为主并将主降级为从`

`repmgr witness register 注册一个观察节点                    `

`repmgr witness unregister 移除一个观察节点                `

`repmgr node status 显示节点的基本信息和复制状态`

`repmgr node check 从复制的角度对节点进行健康监测  `

`repmgr node rejoin 重新加入一个失效节点到集群             `

`repmgr cluster show 显示所有集群中注册的节点信息 `

`repmgr cluster matrix 在所有节点运行show并汇总  `

`repmgr cluster crosscheck 在节点间两两交叉监测连接   `

`repmgr cluster event 输出时间记录               `

`repmgr cluster cleanup 清理监控历史             `

## 查看集群状态

```[postgres@pg16std ~]$ repmgr -f /data/repmgr/repmgr.conf cluster show
 ID | Name    | Role    | Status    | Upstream | Location | Priority | Timeline | Connection string
----+---------+---------+-----------+----------+----------+----------+----------+-------------------------------------------------------------------------
 1  | pg16    | primary | * running |          | default  | 100      | 1        | host=192.168.1.15
node_id=2
 port=5432 user=repmgr dbname=repmgr connect_timeout=2
 2  | pg16std | standby |   running | pg16     | default  | 100      | 1        | host=192.168.1.14 port=5432 user=repmgr dbname=repmgr connect_timeout=2
[postgres@pg16std ~]$ repmgr -f /data/repmgr/repmgr.conf daemon start
NOTICE: executing: "/usr/local/pgsql16.2/bin/repmgrd -f /data/repmgr/repmgr.conf start"
NOTICE: repmgrd was successfully started

```


模拟切换命令：repmgr -f /data/repmgr/repmgr.conf standby switchover --siblings-follow --dry-run
```
[postgres@pg16std ~]$ repmgr -f /data/repmgr/repmgr.conf standby switchover --siblings-follow --dry-run
NOTICE: checking switchover on node "pg16std" (ID: 2) in --dry-run mode
INFO: SSH connection to host "192.168.1.15" succeeded
ERROR: unable to execute "repmgr" on "192.168.1.15"
HINT: check "pg_bindir" is set to the correct path in "repmgr.conf"; current value: (not set)
[postgres@pg16std ~]$ repmgr -f /data/repmgr/repmgr.conf standby switchover --siblings-follow --dry-run
NOTICE: checking switchover on node "pg16std" (ID: 2) in --dry-run mode
INFO: SSH connection to host "192.168.1.15" succeeded
ERROR: unable to execute "repmgr" on "192.168.1.15"
HINT: check "pg_bindir" is set to the correct path in "repmgr.conf"; current value: (not set)
[postgres@pg16std ~]$ repmgr -f /data/repmgr/repmgr.conf standby switchover --siblings-follow --dry-run
NOTICE: checking switchover on node "pg16std" (ID: 2) in --dry-run mode
INFO: SSH connection to host "192.168.1.15" succeeded
ERROR: "data_directory" parameter in "repmgr.conf" on "pg16" (ID: 1) is incorrectly configured
HINT: execute "repmgr node check --data-directory-config" on "pg16" (ID: 1) to diagnose the issue
[postgres@pg16std ~]$ repmgr -f /data/repmgr/repmgr.conf standby switchover --siblings-follow --dry-run
NOTICE: checking switchover on node "pg16std" (ID: 2) in --dry-run mode
INFO: SSH connection to host "192.168.1.15" succeeded
INFO: able to execute "repmgr" on remote host "192.168.1.15"
WARNING: option "--sibling-nodes" specified, but no sibling nodes exist
INFO: 1 walsenders required, 10 available
INFO: demotion candidate is able to make replication connection to promotion candidate
INFO: 0 pending archive files
INFO: replication lag on this standby is 0 seconds
NOTICE: attempting to pause repmgrd on 2 nodes
INFO: would pause repmgrd on node "pg16" (ID: 1)
INFO: would pause repmgrd on node "pg16std" (ID: 2)
NOTICE: local node "pg16std" (ID: 2) would be promoted to primary; current primary "pg16" (ID: 1) would be demoted to standby
INFO: following shutdown command would be run on node "pg16":
  "/usr/local/pgsql/bin/pg_ctl  -D '/data/postgres/data' -W -m fast stop"
INFO: parameter "shutdown_check_timeout" is set to 60 seconds
INFO: prerequisites for executing STANDBY SWITCHOVER are met
```
通过模拟切换发现配置方面的问题，进行处理后，提示模拟切换正常。

查看状态，进行切换

```
[postgres@pg16std ~]$ repmgr -f /data/repmgr/repmgr.conf cluster show
 ID | Name    | Role    | Status    | Upstream | Location | Priority | Timeline | Connection string
----+---------+---------+-----------+----------+----------+----------+----------+-------------------------------------------------------------------------
 1  | pg16    | primary | * running |          | default  | 100      | 1        | host=192.168.1.15 port=5432 user=repmgr dbname=repmgr connect_timeout=2
 2  | pg16std | standby |   running | pg16     | default  | 100      | 1        | host=192.168.1.14 port=5432 user=repmgr dbname=repmgr connect_timeout=2
[postgres@pg16std ~]$ repmgr -f /data/repmgr/repmgr.conf standby switchover --siblings-follow --force-rewind
NOTICE: executing switchover on node "pg16std" (ID: 2)
WARNING: option "--sibling-nodes" specified, but no sibling nodes exist
NOTICE: attempting to pause repmgrd on 2 nodes
NOTICE: local node "pg16std" (ID: 2) will be promoted to primary; current primary "pg16" (ID: 1) will be demoted to standby
NOTICE: stopping current primary node "pg16" (ID: 1)
NOTICE: issuing CHECKPOINT on node "pg16" (ID: 1)
DETAIL: executing server command "/usr/local/pgsql/bin/pg_ctl  -D '/data/postgres/data' -W -m fast stop"
INFO: checking for primary shutdown; 1 of 60 attempts ("shutdown_check_timeout")
INFO: checking for primary shutdown; 2 of 60 attempts ("shutdown_check_timeout")
NOTICE: current primary has been cleanly shut down at location 0/4F000028
NOTICE: promoting standby to primary
DETAIL: promoting server "pg16std" (ID: 2) using pg_promote()
NOTICE: waiting up to 60 seconds (parameter "promote_check_timeout") for promotion to complete
NOTICE: STANDBY PROMOTE successful
DETAIL: server "pg16std" (ID: 2) was successfully promoted to primary
NOTICE: issuing CHECKPOINT on node "pg16std" (ID: 2)
ERROR: unable to execute CHECKPOINT
NOTICE: node "pg16std" (ID: 2) promoted to primary, node "pg16" (ID: 1) demoted to standby
NOTICE: switchover was successful
DETAIL: node "pg16std" is now primary and node "pg16" is attached as standby
NOTICE: STANDBY SWITCHOVER has completed successfully
[postgres@pg16std ~]$ repmgr -f /data/repmgr/repmgr.conf cluster show
 ID | Name    | Role    | Status    | Upstream | Location | Priority | Timeline | Connection string
----+---------+---------+-----------+----------+----------+----------+----------+-------------------------------------------------------------------------
 1  | pg16    | standby |   running | pg16std  | default  | 100      | 1        | host=192.168.1.15 port=5432 user=repmgr dbname=repmgr connect_timeout=2
 2  | pg16std | primary | * running |          | default  | 100      | 2        | host=192.168.1.14 port=5432 user=repmgr dbname=repmgr connect_timeout=2
[postgres@pg16std ~]$
```
可以看到切换之后，pg16 变成了 standby，原从库变成主库：

在原主库上查看：

```
[postgres@pg16 bin]$ repmgr -f /data/repmgr/repmgr.conf cluster show
 ID | Name    | Role    | Status    | Upstream | Location | Priority | Timeline | Connection string
----+---------+---------+-----------+----------+----------+----------+----------+-------------------------------------------------------------------------
 1  | pg16    | standby |   running | pg16std  | default  | 100      | 1        | host=192.168.1.15 port=5432 user=repmgr dbname=repmgr connect_timeout=2
 2  | pg16std | primary | * running |          | default  | 100      | 2        | host=192.168.1.14 port=5432 user=repmgr dbname=repmgr connect_timeout=2
```

修改后的 repmgr.conf 文件如下：
```
[postgres@pg16 bin]$ cat /data/repmgr/repmgr.conf
node_id=1
node_name='pg16'
conninfo='host=192.168.1.15 port=5432 user=repmgr dbname=repmgr connect_timeout=2'
data_directory='/data/postgres/data'
failover='automatic'
promote_command='/usr/local/pgsql16.2/bin/repmgr standby promote --config-file=/data/repmgr/repmgr.conf --log-to-file'
follow_command='/usr/local/pgsql16.2/bin/repmgr standby follow --config-file=/data/repmgr/repmgr.conf --log-to-file --upstream-node-id=%n'
monitoring_history=yes
connection_check_type=ping
reconnect_attempts=6
reconnect_interval=10
standby_disconnect_on_failover=true
repmgrd_pid_file='/data/repmgr/repmgrd.pid'
repmgrd_service_start_command='/usr/local/pgsql16.2/bin/repmgrd -f /data/repmgr/repmgr.conf start'
repmgrd_service_stop_command='kill -9 `cat /data/repmgr/repmgrd.pid`'
log_level=INFO
log_file='/data/repmgr/repmgrd.log'
log_status_interval=10
pg_bindir='/usr/local/pgsql/bin'
data_directory='/data/postgres/data'
```
