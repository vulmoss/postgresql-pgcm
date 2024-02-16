pg sql优化

###优化器

parser=>analyzer=>rewriter=>planner=>excutor

###执行计划

####单表访问方法：

元组ID  ctid ~ oracle rowid

对象id   tid

顺序扫描  Seq Scan

索引扫描  INdex Scan  返回已排序行的方式

仅索引扫描 Index Only Scan 查询中的列都是一个索引中

位图索引扫描 Bitmap scan

####多表连接

hash join 等值查询   ----> 排序情况不适合

Nested loops 其中一边对象非常小，并且包括有限的数据集

Merge join 合并lianjie  两边排序

####统计信息：analyze

create statistics 创建跨列统计信息

PG_STATISTIC 、 PG_STATS 、pg_stat_all_tables 、pg_stat_user_tables







