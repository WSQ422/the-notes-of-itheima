#### 插入
1. 给指定字段添加数据  
`INSERT INTO 表名 (字段1，字段2，...) VALUES (值1，值二，...);`
2. 给全部字段添加数据  
`INSERT INTO 表名 VALUES (值1，值2，...);`
3. 批量添加数据  
`INSERT INTO 表名 (字段1，字段2，...) VALUES (值1，值2，...),(值1，值2，...),(值1，值2，...);`  
`INSERT INTO 表头 VALUES(值1，值2,...)，(值1，值2,...)，(值1，值2,...);`  
注意:字符串和日期型数据应该包含在单引号中。
#### 更新和删除
1. 更改数据
`UPDATE 表名 SET 字段名1 = 值1，字段名2 = 值2(WHERE 条件)；` 
当没有where条件时，会更改整张表的所有数据

1. 删除数据
`DELETE FROM 表名 (WHERE 条件);`  
当没有where条件时，会删除整张表的所有数据
