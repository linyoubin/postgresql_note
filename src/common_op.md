# 常用命令 #

## 定位慢查询 ##

``` shell
select procpid, start, client_host, client_port, now() - start as lap, current_query
from 
  (
     select backendid, pg_stat_get_backend_pid(S.backendid) as procpid, 
            pg_stat_get_backend_activity_start(S.backendid) as start,
            pg_stat_get_backend_client_addr(S.backendid) as client_host,
            pg_stat_get_backend_client_port(S.backendid) as client_port,
            pg_stat_get_backend_activity(S.backendid) as current_query
     from
            (select pg_stat_get_backend_idset() as backendid) as S
  ) as S
order by 
 lap DESC;
```

## 修改表属性 ##

### 增加字段 ###

+ 语法

``` shell
ALTER TABLE table_name ADD column_name datatype; 
```

+ 例子

``` shell
ALTER TABLE test ADD id integer; 
```
