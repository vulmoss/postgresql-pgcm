# oracle_fdw的安装和使用

解压3个oracle zip包  把压缩包放在/home/postgres/下面  并进行解压缩。
  
```
  unzip instantclient-sdk-linux.x64-11.2.0.4.0.zip   unzip instantclient-basic-linux.x64-11.2.0.4.0.zip   unzip instantclient-sqlplus-linux.x64-11.2.0.4.0.zip 
```

```
    cd /home/postgres/instantclient_11_2  ln -s libclntsh.so.11.1 libclntsh.so
```

配置环境变量
  配置 postgres用户的环境变量
  
```
  export ORACLE_HOME=/home/postgres/instantclient_11_2  export OCI_LIB_DIR=$ORACLE_HOME  export OCI_INC_DIR=$ORACLE_HOME/sdk/include  export LD_LIBRARY_PATH=$ORACLE_HOME:$LD_LIBRARY_PATH
```

创建conf  在/etc/ld.so.conf.d/下面创建oracle.conf文件  oracle.conf包含  /home/postgres/instantclient_11_2
  
   ldconfig -v 
   
安装oracle_fdw      
将oracle_fdw-2.6.0.zip放到/home/postgres/下面```
  psql  \c lyradb  create extension oracle_fdw;
```

创建 dblink

```
  CREATE SERVER oracle_server FOREIGN DATA WRAPPER oracle_fdw OPTIONS (dbserver '//192.168.2.103:1521/szhis');      GRANT USAGE ON FOREIGN SERVER oracle_server TO lyra_ops;      CREATE USER MAPPING FOR lyra_ops SERVER oracle_server OPTIONS (user 'zjhis', password 'zjhis');
```

使用dblink注意，下面的schema名称和表名，都用大写，否则会导致查找不到对象。另外把外部表设置为只读，以免误操作，把源端的数据给修改了。推荐用第1种方式，简单方便，如果碰到报错，再考虑用第2种方式，第2种方式，可以指定特定的列。

```
    1. 导入指定表GY_BRJBXXK    IMPORT FOREIGN SCHEMA "ZJHIS" LIMIT TO (GY_BRJBXXK) from server oracle_server into lyra_ops OPTIONS (readonly 'true', prefetch '1000');        2. 排除表GY_BRJBXXK导入    IMPORT FOREIGN SCHEMA "ZJHIS" EXCEPT (GY_BRJBXXK) from server oracle_server into lyra_ops OPTIONS (readonly 'true', prefetch '2000');        3. 全部导入    IMPORT FOREIGN SCHEMA "ZJHIS" from server oracle_server into lyra_ops OPTIONS (readonly 'true', prefetch '2000');  (二)使用方式一：CREATE FOREIGN TABLE    1.手动创建外部表    CREATE FOREIGN TABLE LYRA_OPS.GY_BRJBXXK(code bigint ,name varchar(100) ) SERVER oracle_server OPTIONS (schema 'ZJHIS', table 'GY_BRJBXXK',readonly 'true');        2.设置外部表为只读    alter foreign table GY_BRJBXXK OPTIONS (readonly 'true');
```



```
lyradb=# select * from pg_user_mappings; umid  | srvid |    srvname    | umuser |  usename  |           umoptions           -------+-------+---------------+--------+-----------+------------------------------- 36346 | 36345 | oracle_server |  17556 | lyra_ops  | {user=HRPDEV,password=hrpdev} 36347 | 36345 | oracle_server |  35784 | wain_sjfx | {user=zjhis,password=zjhis}(2 rows)lyradb=# drop user mapping if exists for wain_sjfx server oracle_server;DROP USER MAPPINGlyradb=#  CREATE USER MAPPING FOR wain_sjfx SERVER oracle_server OPTIONS (user 'HRPDEV', password 'hrpdev');CREATE USER MAPPINGlyradb=# select * from pg_user_mappings; umid  | srvid |    srvname    | umuser |  usename  |           umoptions           -------+-------+---------------+--------+-----------+------------------------------- 36346 | 36345 | oracle_server |  17556 | lyra_ops  | {user=HRPDEV,password=hrpdev} 36349 | 36345 | oracle_server |  35784 | wain_sjfx | {user=HRPDEV,password=hrpdev}(2 rows)lyradb=# 
```


整个用户都导入：

```
  GRANT USAGE ON FOREIGN SERVER oracle_server TO wain_sjfx;  CREATE USER MAPPING FOR wain_sjfx SERVER oracle_server OPTIONS (user 'HRPDEV', password 'hrpdev');    GRANT USAGE ON FOREIGN SERVER oracle_server TO lyra_owner;  CREATE USER MAPPING FOR lyra_owner SERVER oracle_server OPTIONS (user 'HRPDEV', password 'hrpdev');    \c lyradb lyra_owner  IMPORT FOREIGN SCHEMA "HRPDEV" from server oracle_server into wain_sjfx OPTIONS (readonly 'true', prefetch '1000');
```



导入表：```
postgres=# \c lyradb lyra_ownerYou are now connected to database "lyradb" as user "lyra_owner".lyradb=> IMPORT FOREIGN SCHEMA "HRPDEV" LIMIT TO (v_hr_zhiwubd) from server oracle_server into wain_sjfx OPTIONS (readonly 'true', prefetch '1000');IMPORT FOREIGN SCHEMAlyradb=> IMPORT FOREIGN SCHEMA "HRPDEV" LIMIT TO (v_hr_zhichengbd) from server oracle_server into wain_sjfx OPTIONS (readonly 'true', prefetch '1000');IMPORT FOREIGN SCHEMAlyradb=> IMPORT FOREIGN SCHEMA "HRPDEV" LIMIT TO (v_hr_keshi) from server oracle_server into wain_sjfx OPTIONS (readonly 'true', prefetch '1000');IMPORT FOREIGN SCHEMA
```

删除旧的 foreign 表，重新导入新的。```
postgres=# \c lyradb lyra_ownerYou are now connected to database "lyradb" as user "lyra_owner".lyradb=> drop foreign table wain_sjfx.v_sjfx_rchx_kaohedjitem;DROP FOREIGN TABLElyradb=> drop foreign table wain_sjfx.v_sjfx_rchx_kaohe;DROP FOREIGN TABLElyradb=> IMPORT FOREIGN SCHEMA "HRPDEV" LIMIT TO (v_sjfx_rchx_kaohedjitem) from server oracle_server into wain_sjfx OPTIONS (readonly 'true', prefetch '1000');IMPORT FOREIGN SCHEMAlyradb=>  IMPORT FOREIGN SCHEMA "HRPDEV" LIMIT TO (v_sjfx_rchx_kaohe) from server oracle_server into wain_sjfx OPTIONS (readonly 'true', prefetch '1000');IMPORT FOREIGN SCHEMAlyradb=> 
```
