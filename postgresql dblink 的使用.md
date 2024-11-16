# postgresql dblink 的使用
dblink_connect[](https://www.postgresql.org/docs/16/contrib-dblink-connect.html)
> dblink_connect — opens a persistent connection to a remote database
首先安装插件
create extension dblink ;

使用方法如下：
```--常规使用
select * from dblink('hostaddr=172.19.180.15 port=5432 dbname=lyradb user=lyra_owner password=lyra_owner##2024','select "id" from "lyra_dz.dz_yh_jibenxx"') AS testTable (id varchar(50));
--在事务中进行数据的修改
1. 先执行dblink_connect保持连接
 SELECT dblink_connect('test','hostaddr=172.19.180.15 port=5432 dbname=lyradb user=lyra_owner password=lyra_owner##2024');
2. 执行BEGIN命令
 SELECT dblink_exec('test', 'BEGIN');
3. 执行数据操作（update，insert，create等命令）
 SELECT dblink_exec('test', 'insert into test1(id) values (1) ');
4. 执行事务提交
 SELECT dblink_exec('test', 'COMMIT');
5. 解除连接
 SELECT dblink_disconnect('test');
```

通过创建视图的方式，持久化的使用 dblink 对其他的库进行访问。
```
CREATE VIEW wain_sjfx.view_15_dz_yh_jibenxx AS 
select * from dblink('hostaddr=172.19.180.15 port=5432 dbname=lyradb user=lyra_owner password=lyra_owner##2024','select id
,zuhuid
,zuhumc
,zuzhijgid
,zuzhijgmc
,yonghuid
,yonghudlm
,yonghuxm
,shenfenzh
,lianxidh
,mima
,jiamiyan
,mimaxgsj
,mimaqzxgbz
,mimayxq
,denglucwcs
,zuijindlsj
,beizhu
,shuruma1
,shuruma2
,shuruma3
,shunxuhao
,chuangjianrid
,chuangjianrxm
,chuangjianxtid
,chuangjiansj
,xiugairid
,xiugairxm
,xiugaixtid
,xiugaisj
,zuofeibz
,zhigongid
,chaoguanbz
,qiyongbz
,shoujihao
,youxiang
,biangengbb
,xingbiedm
,xingbiemc
,qianmingjg from lyra_dz.dz_yh_jibenxx') AS testTable(id varchar(50)
,zuhuid varchar(50)
,zuhumc varchar(200)
,zuzhijgid varchar(50)
,zuzhijgmc varchar(200)
,yonghuid varchar(50)
,yonghudlm varchar(50)
,yonghuxm varchar(50)
,shenfenzh varchar(100)
,lianxidh varchar(50)
,mima varchar(100)
,jiamiyan varchar(50)
,mimaxgsj timestamp(6)
,mimaqzxgbz int2
,mimayxq int8
,denglucwcs int4
,zuijindlsj timestamp(6)
,beizhu varchar(500)
,shuruma1 varchar(20)
,shuruma2 varchar(20)
,shuruma3 varchar(20)
,shunxuhao int4
,chuangjianrid varchar(20)
,chuangjianrxm varchar(50)
,chuangjianxtid varchar(20)
,chuangjiansj timestamp(6)
,xiugairid varchar(20)
,xiugairxm varchar(50)
,xiugaixtid varchar(20)
,xiugaisj timestamp(6)
,zuofeibz int2
,zhigongid varchar(20)
,chaoguanbz int2
,qiyongbz int2
,shoujihao varchar(100)
,youxiang varchar(50)
,biangengbb varchar(50)
,xingbiedm varchar(50)
,xingbiemc varchar(50),qianmingjg varchar(500)) ;
```
