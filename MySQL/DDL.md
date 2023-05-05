#### 数据库操作
+ 查询所有数据库  
 ` SHOW DATABASES;`
+ 查询当前数据库  
 ` SELECT DATABASE();`
+ 创建  
 ` CREATE DATABASE (IF NOT EXISTS) 数据库名字 (DEFAULT CHARSET 字符集) (COLLATE 排序规则)；`
+ 删除  
 ` DROP DATABASE (IF EXISTS) 数据库名字；`
+ 使用  
  `USE 数据库名字；`
#### 表操作
### 查询
+ 查询当前数据库所有表  
 ` SHOW TABLES;`
+ 查询表结构  
 ` DESC 表名；`
+ 查询指定表的建表语句  
  `SHOW CREATE TABLE 表名；`
### 创建
```
CREATE TABLE 表名( 
    字段1 字段1类型 (COMMENT 字段1注释)， 
    字段2 字段2类型 (COMMENT 字段2注释)， 
    字段3 字段3类型 (COMMENT 字段3注释)，  
    ...   
    字段n 字段n类型 (COMMENT 字段n注释)  
)(COMMENT 表注释);
```
### 数据类型
1. 数值类型(加上UNSIGNED表示无符号数值)
+ `TINYINT` 小整数值
+ `SMALLINT `大整数值
+ `MEDIUMINT` 大整数值
+ `INT 或 INTEGER` 大整数值
+ `BIGINT `极大整数值
+ `FLOAT` 单精度浮点型
+ `DOUBLE` 双精度浮点型
+ `DECIMAL` 小数值 decimal(长度，精度)
2. 字符串类型
+ `CHAR` 定长字符串
+ `VARCHAR` 变长字符串
+ `TITYBLOB` 不超过255个字符的二进制数据
+ `TITYTEXT` 短文本字符串
+ `BLOB` 二进制形式的长文本数据
+ `TEXT` 长文本数据
+ `MEDIUMBLOB` 二进制形式的中等长度文本数据
+ `MEDIUMTEXT` 中等长度文本数据
+ `LONGBLOB` 二进制形式的极大文本数据
+ `LONGTEXT` 极大文本数据
3. 日期类型
+ `DATA` 日期值
+ `TIME` 时间值或持续时间
+ `YEAR` 年份值
+ `DATETIME` 日期和时间值
+ `TIMESTAMP` 日期和时间值和时间戳
### 修改
+ 添加字段  
`ALTER TABLE 表名 ADD 字段名 类型（长度） （COMMENT 注释）；`
+ 修改数据类型  
`ALTER TABLE 表名 MODIFY 字段名 新数据类型（长度）；`
+ 修改字段名和字段类型  
`ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型（长度）（COMMENT 注释）； `
+ 删除字段  
`ALTER TABLE 表名 DROP 字段名；`
+ 修改表名  
`ALTER TABLE 表名 RENAME TO 新表名`
+ 删除表  
`DROP TABLE (IF EXISTS)表名；`
+ 删除指定表，并重新创建该表  
`TRUNCATE TABLE 表名； `