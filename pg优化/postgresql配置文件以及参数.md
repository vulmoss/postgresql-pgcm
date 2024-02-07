#postgresql配置文件：

internal

postmaster

sighup

backend

superuser-backend

user

supseruser

参数生效是否重启









hda.conf

连接参数



配置调优参数

PG双缓冲

1.shared_buffers(RAM 1/4)

2.work_mem (ram 1/4  / max_connections) 复杂排序 一个连接占用一个

3.maintenance_work_mem （ram * 0.15 / autovacuum_max_workers）维护操作时需要的内存

4.wal_buffers= 16GB

random_page_cost=1.1~3 (>seq_page_cost:1) 随机磁盘访问时单个页面的读取开销，默认为4.0

work_mem（ram # 0.25 /max_cinnections）

effective_cache_size=(ram # 3/4)

wal_level=replica

synchronous_commit=(on,local,off,remote_write,remote_apply,off)

checkpoint_timeout=5min~15

checkpoint_completeion_target=

checkpoint_warning

高并发事务提交：

wal_writer_delay=10ms wal写入器刷写wal的频繁程度

commit_delay=10ms 事务提交后，日志写入wal_buffer上到wwal_buffers写到磁盘的时间间隔 （需要和comit_sibling配合）

####autovacuum：

死元祖清理调优:idle_in_transaction_session_timeout, statement_timeout

加快autovacuum运行:autovacuum_vacuum_sot_limit ,autovacuum_vacuum_cost_delay

降低死元祖生成速度:多条update合并为一条

单表修改数据调优：autovacuum_vacuum_scale_factor

避免事务ID回卷：

表分区增大并workers



#优化工具

PGTune

postgresqltuner

pgBadger