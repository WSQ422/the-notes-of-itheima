#### 基本查询
1. 查询多个字段
`SELECT 字段1，字段2，字段3...FROM 表名;`  
`SELECT * FROM 表名;`
2. 设置别名
`SELECT 字段1 (AS 别名1)，字段2 (AS 别名2)... FROM 表名；`(AS可以省略)
3. 去除重复记录
`SELECT DISTINCT 字段列表 FROM 表名;`
#### 条件查询
1. 语法
`SELECT 字段列表 FROM 表名 WHERE 条件列表`
2. 条件  
| 比较运算符 | 功能 |  
| :----: | :----: |
|>|大于|