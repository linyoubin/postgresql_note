# null 值 #

在 PostgreSQL 中，字段中的 null 比较特殊。

+ null 之间不能使用 <>, = 作比较

    ``` shell
    postgres=# select null <> null ,null = null ,null is null,null is not null;
    ?column? | ?column? | ?column? | ?column? 
    ----------+----------+----------+----------
              |          | t        | f

    ```

    null 之间使用 <>  = 比较结果未定义，既不是 t (true)，也不是 f (false)。

+ null 与字段之间不能使用 <>, = 作比较

    ``` shell
    postgres=# create table null1(id integer, name text);
    postgres=# insert into null1 values(1);
    postgres=# select * from null1;
     id | name
    ----+------
      1 |
    (1 rows)

    postgres=# select * from null1 where name = null;
     id | name
    ----+------
    (0 rows)

    postgres=# select * from null1 where name is null;
     id | name
    ----+------
      1 |
    (1 rows)
    ```

+ 表关联字段中有 null 值时无法匹配

    ``` shell
    postgres=# create table null1(id integer, name text);
    postgres=# insert into null1 values(1);
    postgres=# select * from null1 where id = 1;
     id | name
    ----+------
      1 |
    (1 rows)

    postgres=# select * from null1 where id = 1;
     id | name
    ----+------
      1 |
    (1 rows)

    postgres=# select * from null1 where id = 1;
     id | name
    ----+------
      1 |
    (1 rows)

    postgres=# select * from null1 as t1, null2 as t2 where t1.name = t2.name and t1.id = 1;
     id | name | id | name
    ----+------+----+------
    (0 rows)
    ```

