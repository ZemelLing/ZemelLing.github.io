---
title: "MySql的用法记录"
date: 2021-04-06T22:12:49+08:00
draft: false
tags: ["mysql", ]
categories: ["mysql", ]
---

## MySql 中的DataType

|类型|含义|范围|
|----|----|----|
|数值类型|||
|INT|整数|-2147483647 ~ 2147483647|
|TINYINT|极小整数|-128 ~ 127|
|SMALLINT|小整数|-32768 ~ 32767|
|MEDIUMINT|中等整数|-8388608 ~ 8388607|
|BIGINT|大整数|-9223372036854775808 ~ 9223372036854775807|
|FLOAT|单精度浮点数|-3.402823466E+38 ~ -1.175494351E-38  0  1.175494351E-38 ~ 3.402823466E+38|
|DOUBLE|双精度浮点数|-1.7976931348623157E+308 ~ -2.2250738585072014E-308  0  2.2250738585072014E-308 ~ 1.7976931348623157E+308|
|DECIMAL|精确小数|DECIMAL(最大位数，小数点后的位数) 最大位数可指定不大于65的值，小数点之后的位数不大于30|
|字符串类型|||
|CHAR|固定长度字符串|超过255个字符|
|VAECHAR|可变长字符串|1 ~ 65532字节。字符数的上限取决于字符编码|
|TEXT|长文本字符串|长度不超过65535个字符|
|LONGTEXT|超长文本字符串|长度不超过4294967295个字符|
|日期与时间类型|||
|DATETIME|日期与时间|1000-01-01 00:00:00 ~ 9999-12-31 23:59:59|
|DATE|日期|1000-01-01 ~ 9999-12-31|
|YEAR|年|1901 ~ 2155（4 位时）  1970 ~ 2069（70 ~ 69）（2 位时）|
|TIME|时间|-838:59:59 ~ 838:59:59|

***注意：*** 日期必须以 YYYY-MM-DD 的格式输入，时间必须以 HH:MM:SS 的格式输入。

## 简单使用

* 不区分大小写

### 创建数据库

```sql
CREATE DATABASE databaseName;

CREATE DATABASE databaseName CHARACTER SET utf8 COLLATE utf8_general_ci;
```

***注意:*** 数据库名区分大小写

### 显示数据库列表

```sql
SHOW DATABASES;
```

### 指定使用的数据库

```sql
use databaseName # 指定使用的数据库,此条语句不是 SQL 语句。

SELECT DATABASE(); # 当前使用的数据库
```

### 创建表

```sql
CREATE TABLE tableName ( columnName1 dataType1, columnName2 dataType2,columnName3 dataType3, ...); # 无法替换同名表

CREATE TABLE tableName ( columnName1 dataType1, columnName2 dataType2,columnName3 dataType3, ...) CHARSET=utf8; # CHAESET 指定数据表的字符集

CREATE TABLE tableName ( columnName1 dataType1 PRIMARY KEY, columnName2 dataType2,columnName3 dataType3, ...); # 创建表时同时指定主键
```

***注意：*** 数据库名、表名、列名可用反引号 **``** 包裹起来。

### 显示所有表

```sql
SHOW TABLES;
```

***注意：*** 可以通过将数据库名与表名用点号 **.** 连接，从而访问和操作其它数据库的内容。

### 显示表结构

```sql
DESC tableName;
```

### 向表中插入记录

```sql
INSERT INTO tableName VALUES (data1, data2, data3); # 按照列顺序插入数据

INSERT INTO tableName (columnName1, columnName2, ...) VALUES (data1, data2, ...); # 指定ColumnName插入数据，此时列顺序无关紧要

INSERT INTO tableName (columnName1, columnName2, ...) VALUES (data1, data2, ...), (data1, data2, ...),...; # 一次性插入多行数据, 需要确保关键字在一行，单行数据也要保持在一行，如果需要换行输入的话
```

***注意：*** 字符串数据需要用双引号 **""** 或单引号 **''** 包裹起来。

### 显示数据

```sql
SELECT columnName1, columnName2, ... FROM tableName; # 显示表中指定列的数据

SELECT * FROM tableName; # 显示表中所有数据

SELECT 'Test'; # 输出与数据库无关的值
```

## 修改表

### 修改表结构

* 修改列定义：ALTER TABLE ... MODIFY ...
* 添加列：ALTER TABLE ... ADD ...
* 修改列名和定义：ALTER TABLE ... CHANGE ...
* 删除列：ALTER TABLE ... DROP ...

### 修改列数据类型

```sql
ALTER TABLE tableName MODIFY columnName VARCHAR(100); # 将 tableName 表的 columnName 列的类型改为 VARCHAR(100)
```

### 增加列

```sql
ALTER TABLE tableName ADD columnName dataType; # 添加在表的末尾

ALTER TABLE tableName ADD columnName dataType FIRST; # 添加在表的前面

ALTER TABLE tableName ADD columnName dataType AFTER anotherColumnName; # 添加在表的 anotherColumnName 列的后边
```

### 更改列的位置

```sql
ALTER TABLE tableName MODIFY columnName dataType FIRST; # 修改到表的前面

ALTER TABLE tableName MODIFY columnName dataType AFTER anotherColumnName; # 修改到 anotherColumnName 列的后边
```

### 修改列名同时修改数据类型

```sql
ALTER TABLE tableName CHANGE columnNameBeforeModify columnNameAfterModify dataTypeAfterModify; # 也可以同时修改列约束
```

### 删除列

```sql
ALTER TABLE tableName DROP columnName;
```

### 设置主键

主键（Primary Key）特点：

* 不允许重复
* 不允许为空

```sql
CREATE TABLE tableName ( columnName1 dataType1 PRIMARY KEY, columnName2 dataType2,columnName3 dataType3, ...); # 创建表时同时指定主键

ALTER TABLE tableName DROP PRIMARY KEY; # 删除主键

ALTER TABLE 表名 ADD PRIMARY KEY (columnName1, columnName2, ...);  # 添加主键

```

### 设置唯一键

***注意：*** 唯一键（Unique Key）不是主键

```sql
CREATE TABLE tableName ( columnName1 dataType1 UNIQUE, columnName2 dataType2,columnName3 dataType3, ...); # 创建表时同时指定唯一键

ALTER TABLE tableName DROP INDEX columnName; # 移除唯一键

ALTER TABLE tableName ADD UNIQUE (columnName); # 添加唯一键
```

### 设置列自动编号

***注意：*** 设置自动编号的列具备以下特点
* 为整型类型
* 需要 AUTO_INCREMENT 约束
* 需要有 PRIMARY KEY 约束

```sql
CREATE TABLE tableName (columnName1 dataType1 PRIMARY KEY AUTO_INCREMENT, columnName2 dataType2); # 在创建表时指定自动编号, dataType1 要为整型

ALTER TABLE tableName MODIFY columnName dataType PRIMARY KEY AUTO_INCREMENT;

ALTER TABLE tableName AUTO_INCREMENT=1; # 重新从1开始编号
```

### 设置列的默认值

```sql
CREATE TABLE tableName (columnName1 dataType1 DEFAULT defaultValue, columnName2 dataType2); # 在创建表时指定列的默认值

ALTER TABLE tableName MODIFY columnName dataType DEFAULT defaultValue; # 设置或修改列的默认值
```

### 通过 MODIFY 变更约束

```sql
ALTER TABLE tableName MODIFY columnName dataType NULL; # 设置列可空，NULL 可以替换为其他约束
```

### 索引操作

***注意：*** 在设置主键后，会自动创建索引

```sql
CREATE INDEX indexName ON tableName (columnName); # 创建索引

SHOW INDEX FROM tableName; # 显示索引

DROP INDEX indexName ON tableName; # 删除索引
```

## 遇到的错误

### 错误1 Statement violates GTID consistency: CREATE TABLE ... SELECT.

#### 错误原因

这是因为在5.6及以上的版本内，开启了 enforce_gtid_consistency=true 功能导致的，MySQL官方解释说当启用 enforce_gtid_consistency 功能的时候，MySQL只允许能够保障事务安全，并且能够被日志记录的SQL语句被执行，像create table … select 和 create temporarytable语句，以及同时更新事务表和非事务表的SQL语句或事务都不允许执行。

#### 解决方法

* 方法1（推荐）

修改 ：SET @@GLOBAL.ENFORCE_GTID_CONSISTENCY = off;

配置文件中 ：ENFORCE_GTID_CONSISTENCY = off;

* 方法2

