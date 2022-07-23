
当然对于数据的操作，通常是放在代码里的，所以需要用相关的语句才行。SQL应运而生，它并非什么独立的编程语言，相反更类似于shell的操作。

对于数据库的语法而言，无非是一些对内部数据，数据表，数据库的操作。归结起来分为四大类，增删改查。从关键字来分类描述。

# help

```sql

\help SELECT //查看SELECT的使用语法

```

# 增

## CREATE

```sql

CREATE DATABASE databasename;       -- 新增数据库--databasename

CREATE TABLE tablename;             -- 新增数据表

CREATE TABLE weather (             -- 带字段
    city            varchar(80),
    temp_lo         int,           -- 最低温度
    temp_hi         int,           -- 最高温度
    prcp            real,          -- 湿度
    date            date           -- 时期
);

```

## INSERT

```sql

--往数据表中插入数据

--插入数据按照字段顺序
--不是简单的数值类型的value，需要用''包起来。
INSERT INTO weather VALUES ('San Francisco', 46, 50, 0.25, '1994-11-27');

--但如果只记得有哪些值，但是不记得顺序？可以在插入时设定

INSERT INTO weather (city, temp_lo, temp_hi, prcp, date)
    VALUES ('San Francisco', 43, 57, 0.0, '1994-11-29');

--当然可以用其他的顺序，或者忽略某些列
--改变了字段的顺序，忽略了湿度
INSERT INTO weather (date, city, temp_hi, temp_lo)
    VALUES ('1994-11-29', 'Hayward', 54, 37);

```

# 改


# 删
## DROP

```sql

DROP TABLE tablename;               --删除数据表

```


# 查

## SELECT