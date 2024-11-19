# postgresql表转储到kingbase

pg_dump,original repo🔗 [pg_dump](https://www.postgresql.org/docs/16/app-pgdump.html)

> pg_dump — extract a PostgreSQL database into a script file or other archive file


<u>从pg导出，导入导kingbase，单独导出某个表SQL如下</u>




```
pg_dump -h 127.0.0.1 -U postgres -p 5432 -t lyra_wm.wm_mb_mobannr  --no-publications --no-subscriptions -x  --inserts  --rows-per-insert=1000   lyradb   -f /home/postgres/tmp/wmbnr.sql    > /home/postgres/tmp/vmmnr.log&
```
pg_dump命令参数有如下：


>  -o, --oids                   在转储中包括 OID
> 
>   -O, --no-owner               在明文格式中, 忽略恢复对象所属者
>  
>   -s, --schema-only            只转储模式, 不包括数据
> 
>   -S, --superuser=NAME         在明文格式中使用指定的超级用户名
> 
>   -t, --table=TABLE            只转储指定名称的表
> 
>   -T, --exclude-table=TABLE    不转储指定名称的表
> 
>   -x, --no-privileges          不要转储权限 (grant/revoke)
> 
>   --binary-upgrade             只能由升级工具使用
> 
>   --column-inserts             以带有列名的INSERT命令形式转储数据
> 
>   --disable-dollar-quoting     取消美元 (符号) 引号, 使用 SQL 标准引号
> 
>   --disable-triggers           在只恢复数据的过程中禁用触发器
> 
>   --enable-row-security        启用行安全性(只转储用户能够访问的内容)
> 
>   --exclude-table-data=TABLE   不转储指定名称的表中的数据
> 
>   --if-exists              当删除对象时使用IF EXISTS
> 
> --rows-per-insert=nrows
>将数据转储为INSERT命令（而不是COPY）。控制每个INSERT命令的最大行数。指定的值必须是大于零的数字。还原过程中的任何错误都只会导致有问题的INSERT中的行丢失，而不是整个表内容丢失.

```
[kingbase@db01 bak]$ ksql -p5432 -Ulyra_owner -d lyradb  -f /bak/wmbnr.sql > /bak/imp1.logPassword for user lyra_owner:ksql:/bak/wmbnr.sql:47: ERROR:  relation "wm_mb_mobannr" already existsksql:/bak/wmbnr.sql:718676: ERROR:  multiple primary keys for table "wm_mb_mobannr" are not allowedksql:/bak/wmbnr.sql:718683: ERROR:  relation "idx_wm_mb_mobannr_mbid" already exists[kingbase@db01 bak]$[kingbase@db01 bak]$[kingbase@db01 bak]$ ls -ltrtotal 3383824-rw-r--r--. 1 root     root     3465031663 Nov 16 15:24 wmbnr.sql-rw-rw-r--. 1 kingbase kingbase        454 Nov 16 15:52 imp1.log[kingbase@db01 bak]$ more imp1.logSETSETSETSETSET set_config------------(1 row)SETSETSETSETSETSETALTER TABLECOMMENTCOMMENTCOMMENTCOMMENTCOMMENTCOMMENTCOMMENTCOMMENTCOMMENTCOMMENTCOMMENTCOMMENTCOMMENTCOMMENTCOMMENTCOMMENTCOMMENTCOMMENTCOMMENTINSERT 0 1000INSERT 0 1000INSERT 0 1000INSERT 0 1000INSERT 0 1000INSERT 0 1000INSERT 0 1000INSERT 0 1000INSERT 0 1000INSERT 0 1000INSERT 0 1000INSERT 0 1000INSERT 0 1000INSERT 0 1000INSERT 0 913[kingbase@db01 bak]$
```


<u>postgresql 导出索引和约束</u>：
```
pg_dump -h 127.0.0.1 -Upostgres -p 5432 -W -n merc_ys -n merc_yh  -n merc_hl -n merc_zf  -x   -s --inserts --no-publications --no-subscriptions lyradb -f /tmp/db4.sql > /tmp/db4.logcat db4.sql |grep -i primary -B 1 -A 1 > db4_primary_constraint.sqlcat db4.sql |grep -i index -B 1  -A 1 > db4_index.sql
```


<u>依次导入到人大金仓的库中</u>：

```
ksql -p5432 -Ulyra_owner -d lyradb  -f /home/kingbase/tmp/db4_primary_constraint.sql >  /home/kingbase/tmp/impp4.logksql -p5432 -Ulyra_owner -d lyradb  -f /home/kingbase/tmp/db4_index.sql >  /home/kingbase/tmp/impi4.log
```
