# MySQL 必知必会

## 第 1 章：了解 SQL

1. **数据库（database）** 保存有组织的数据的容器（通常是一个文件或一组文件）。

   > 我们用的通常是数据库软件应称为 DBMS（数据库管理系统）。MySQL 就是一种 DBMS。数据库是通过 DBMS 创建和操纵的容器。数据库可以是保存在硬设备上的文件，但也可以不是。

2. **表（table）** 某种特定类型数据的结构化清单。

   > 同一数据库的表名具有唯一性。
   >
   > 模式（schema） 关于数据库和表的布局及特性的信息。模式可以用来描述数据库中特定的表以及整个数据库（和其中表的关系）。

3. **列（column）** 表中的一个字段

   > **数据类型（datatype）** 所容许的数据的类型。每个表列都有相 应的数据类型，它限制（或容许）该列中存储的数据。

4. **行（row）** 表中的一个记录。

5. 主键（primary key）一列（或一组列），其值能够唯一区分表中每个行。主键要唯一。可以是一个，也可以是一组

   > 主键的好习惯：
   >
   > - 不更新主键列中的值；
   > - 不重用主键列的值；
   > - 不在主键列中使用可能会更改的值（主键最好不要更改）

6. SQL（发音为字母 S-Q-L 或**sequel**）是结构化查询语言（**Structured Query Language**）的缩写。SQL 是一种专门用来与数据库通信的语言。

   > SQL 不是一种专利语言，而且存在一个标准委员会，他们试图定义可供所有 DBMS 使用的 SQL 语法，但事实上任意两个 DBMS 实现的 SQL 都不完全相同。

## 第 2 章：MySQL 简介

1. MySQL 是一种 DBMS，即它是一种数据库软件

2. MySQL 数据库是基于客户机—服务器的数据库。

   > DBMS 可分为两类：一类为基于共享文件系统的 DBMS，另一类为基于客户机—服务器的 DBMS
   >
   > > 共享文件系统的 DBMS: Microsoft Access 和 FileMaker，用于桌面用途
   > >
   > > 客户机—服务器的 DBMS：MySQL、Oracle 以及 Microsoft SQL Server
   >
   > 服务器部分是负责所有数据访问和处理的一个软件。这个软件运行在称为**数据库服务器**的计算机上。
   >
   > 客户机是与用户打交道的软件

3. MySQL 工具

   > 简单命令行实用程序 : mysql
   >
   > 图形交互客户机: MySQL Administrator，MySQL Query Browser

## 第 3 章：使用 MySQL

1. 启动 MySQL

   - 在 Windows 环境下

     - 通过管理员运行 cmd：win+R >> cmd >>> Ctrl+Shift+Enter

     - net start mysql 用这行命令的时候报如下错误

       ```powershell
       C:\Windows\System32>net start mysql
       服务名无效。

       请键入 NET HELPMSG 2185 以获得更多的帮助。
       ```

     - 这样因为服务名错了

       ![](img\001.png)

       net start mysql80 就可以正常启动

       ```powershell
       C:\Windows\System32>net start mysql80
       请求的服务已经启动。

       请键入 NET HELPMSG 2182 以获得更多的帮助。
       ```

2. MySQL 的登录: mysql -u root -p

   ```powershell
   C:\Windows\System32>mysql -u root -p
   Enter password: ****
   Welcome to the MySQL monitor.  Commands end with ; or \g.
   Your MySQL connection id is 1596
   Server version: 8.0.28 MySQL Community Server - GPL

   Copyright (c) 2000, 2022, Oracle and/or its affiliates.

   Oracle is a registered trademark of Oracle Corporation and/or its
   affiliates. Other names may be trademarks of their respective
   owners.

   Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

   mysql>
   ```

3. 创建数据库: create database 数据名
   ```powershell
   mysql> create database temp;
   Query OK, 1 row affected (0.01 sec)
   ```
4. 选择数据库: use 数据库名
   ```powershell
   mysql> use temp
   Database changed
   ```
5. 了解数据库和表：show
   - 了解数据库：mysql> SHOW DATABASES;
   - 了解数据库表：mysql> SHOW TABLES;
   - 了解数据库的字段：
     - mysql> show columns from customers;
     - mysql> describe customers;
     - mysql> show full columns from customers;
   - 了解更多关于 show 的内容：HELP SHOW

## 第 4 章：检索数据 SELECT

1. 单列：SELECT prod_name FROM products;

   - 查询出来的数据是未经过排序的
   - SQL 不区分大小写，作者倾向：对所有 SQL 关键字使用大写，而对所有列和表名使用小写，这样做使代码更易于阅读和调试。
   - 在处理 SQL 语句时，其中所有空格都被忽略，但作者倾向保留空格，便于阅读和调试

2. 多列：SELECT prod_id, prod_name, prod_price FROM products;

3. 使用通配符：SELECT \* FROM products;
   - 作者不建议使用通配符（\*）：检索不需要的列通常会降低检索和应用程序的性能
   ```powershell
      mysql> SELECT * FROM products;
      +---------+---------+----------------+------------+----------------------------------------------------------------+
      | prod_id | vend_id | prod_name      | prod_price | prod_desc                                                      |
      +---------+---------+----------------+------------+----------------------------------------------------------------+
      | ANV01   |    1001 | .5 ton anvil   |       5.99 | .5 ton anvil, black, complete with handy hook                  |
      | ANV02   |    1001 | 1 ton anvil    |       9.99 | 1 ton anvil, black, complete with handy hook and carrying case |
      | ANV03   |    1001 | 2 ton anvil    |      14.99 | 2 ton anvil, black, complete with handy hook and carrying case |
      | DTNTR   |    1003 | Detonator      |      13.00 | Detonator (plunger powered), fuses not included                |
      | FB      |    1003 | Bird seed      |      10.00 | Large bag (suitable for road runners)                          |
      | FC      |    1003 | Carrots        |       2.50 | Carrots (rabbit hunting season only)                           |
      | FU1     |    1002 | Fuses          |       3.42 | 1 dozen, extra long                                            |
      | JP1000  |    1005 | JetPack 1000   |      35.00 | JetPack 1000, intended for single use                          |
      | JP2000  |    1005 | JetPack 2000   |      55.00 | JetPack 2000, multi-use                                        |
      | OL1     |    1002 | Oil can        |       8.99 | Oil can, red                                                   |
      | SAFE    |    1003 | Safe           |      50.00 | Safe with combination lock                                     |
      | SLING   |    1003 | Sling          |       4.49 | Sling, one size fits all                                       |
      | TNT1    |    1003 | TNT (1 stick)  |       2.50 | TNT, red, single stick                                         |
      | TNT2    |    1003 | TNT (5 sticks) |      10.00 | TNT, red, pack of 10 sticks                                    |
      +---------+---------+----------------+------------+----------------------------------------------------------------+
      14 rows in set (0.00 sec)
   ```
4. 检索不同行：DISTINCT-去重

   - SELECT DISTINCT vend_id FROM products;
     ```powershell
          mysql> SELECT DISTINCT vend_id FROM products;
          +---------+
          | vend_id |
          +---------+
          |    1001 |
          |    1002 |
          |    1003 |
          |    1005 |
          +---------+
     ```
   - 不能部分使用 DISTINCT: DISTINCT 关键字应用于所有列而不仅是前置它的列
     ```powershell
        mysql> SELECT DISTINCT vend_id,prod_price FROM products;
        +---------+------------+
        | vend_id | prod_price |
        +---------+------------+
        |    1001 |       5.99 |
        |    1001 |       9.99 |
        |    1001 |      14.99 |
        |    1003 |      13.00 |
        |    1003 |      10.00 |
        |    1003 |       2.50 |
        |    1002 |       3.42 |
        |    1005 |      35.00 |
        |    1005 |      55.00 |
        |    1002 |       8.99 |
        |    1003 |      50.00 |
        |    1003 |       4.49 |
        +---------+------------+
        12 rows in set (0.00 sec)
     ```
     vend_id 和 prod_price 要合起来不一致

5. 限制结果: limit

   select \* from table_name limit 5;

   - 限制返回结果不多于 5 行
     select \* from table_name limit 3,4;
   - 从第 3 行开始，查询 4 条记录
   - 检索出的第一条记录为第 0 行，而不是第 1 行
     select \* from table_name limit 4 offset 3
   - 从第 3 行开始，查询 4 条记录
   - 等同于：select \* from table_name limit 3,4;

6. 使用完全限定的表名

   select \* from 数据库名.表名

## 第 3 章：排序检索数据 ORDER BY

1. 为什么要排序：如果不排序，数据一般将以它在底层表中出现的顺序显示。这可以是数据最初添加到表中的顺序。但是，如果数据后来进行过更新或删除，则此顺序将会受到 MySQL 重用回收存储空间的影响。因此，如果不明确控制的话，不能（也不应该）依赖该排序顺序。

2. **子句（clause）** SQL 语句由子句构成，有些子句是必需的，而有的是可选的。一个子句通常由一个关键字和所提供的数据组成。子句的例子有 SELECT 语句的 FROM 子句，

3. ORDER BY

```sql
   mysql> SELECT prod_id,prod_price,prod_name FROM products ORDER BY prod_price DESC , prod_name ASC;
   +---------+------------+----------------+
   | prod_id | prod_price | prod_name      |
   +---------+------------+----------------+
   | JP2000  |      55.00 | JetPack 2000   |
   | SAFE    |      50.00 | Safe           |
   | JP1000  |      35.00 | JetPack 1000   |
   | ANV03   |      14.99 | 2 ton anvil    |
   | DTNTR   |      13.00 | Detonator      |
   | FB      |      10.00 | Bird seed      |
   | TNT2    |      10.00 | TNT (5 sticks) |
   | ANV02   |       9.99 | 1 ton anvil    |
   | OL1     |       8.99 | Oil can        |
   | ANV01   |       5.99 | .5 ton anvil   |
   | SLING   |       4.49 | Sling          |
   | FU1     |       3.42 | Fuses          |
   | FC      |       2.50 | Carrots        |
   | TNT1    |       2.50 | TNT (1 stick)  |
   +---------+------------+----------------+
```

- ORDER BY A B : 想根据 A 排序，A 有相同的值，就再根据 B 排序
- DESC（descending）降序；ASC(ASCENDING)升序
  - 只作用于其前面的字段
  - prod_price DESC , prod_name ASC ：prod_price 降序；prod_name 升序
- ORDER BY + LIMIT 找到最大值或者最小值
  - SELECT prod_id,prod_price,prod_name FROM products ORDER BY prod_price DESC LIMIT 1;
  - ORDER BY 子句 位于 FROM 子句之后
  - LIMIT 子句位于 ORDER BY 子句之后
- 区分大小写和排序顺序：
  - 这个问题的答案取决于数据库如何设置
  - A 和 a 相同 是 MySQL 的默认行为
  - 也可以通过配置数据库，改变这个默认行为

## 第 6 章：过滤数据 —— 单个 where 条件

1. 一个例子
   ```sql
      mysql> SELECT prod_name, prod_price FROM products WHERE prod_name = 'fuses';
      +-----------+------------+
      | prod_name | prod_price |
      +-----------+------------+
      | Fuses     |       3.42 |
      +-----------+------------+
      1 row in set (0.00 sec)
   ```
   - MySQL 在执行匹配时默认不区分大小写
   - SQL 过滤与应用过滤
     - 应用过滤就是全部查出来，然后循环遍历出需要的值
     - 作者不建议应用过滤
       - SQL 过滤效率更高
       - 服务器传输更多的数据，导致带宽的浪费
   - WHERE 子句的位置：FROM 之后，ORDER BY 之前
2. WHERE 子句操作符
   ```txt
   = 等于
   <> 不等于
   != 不等于
   < 小于；<= 小于等于
   > 大于；>= 大于等于
   BETWEEN a AND b: [a,b]
   IS NULL  不为NULL
   ```
3. BETWEEN
   ```sql
      mysql> SELECT prod_name, prod_price FROM products WHERE prod_price BETWEEN 5.99 AND 10.00;
      +----------------+------------+
      | prod_name      | prod_price |
      +----------------+------------+
      | .5 ton anvil   |       5.99 |
      | 1 ton anvil    |       9.99 |
      | Bird seed      |      10.00 |
      | Oil can        |       8.99 |
      | TNT (5 sticks) |      10.00 |
      +----------------+------------+
      5 rows in set (0.00 sec)
   ```
4. NULL 与不匹配
   ```sql
      -- customers表中有如下数据
      mysql> SELECT cust_id, cust_name, cust_email FROM customers;
      +---------+----------------+---------------------+
      | cust_id | cust_name      | cust_email          |
      +---------+----------------+---------------------+
      |   10001 | Coyote Inc.    | ylee@coyote.com     |
      |   10002 | Mouse House    | NULL                |
      |   10003 | Wascals        | rabbit@wascally.com |
      |   10004 | Yosemite Place | sam@yosemite.com    |
      |   10005 | E Fudd         | NULL                |
      +---------+----------------+---------------------+
      5 rows in set (0.00 sec)
      -- 如果我们我们希望找出cust_email不为ylee@coyote.com 的数据，预期应该有4条数据
      mysql> SELECT cust_id, cust_name, cust_email FROM customers where cust_email <> 'ylee@coyote.com';
      +---------+----------------+---------------------+
      | cust_id | cust_name      | cust_email          |
      +---------+----------------+---------------------+
      |   10003 | Wascals        | rabbit@wascally.com |
      |   10004 | Yosemite Place | sam@yosemite.com    |
      +---------+----------------+---------------------+
      2 rows in set (0.00 sec)
      -- 但实际上只出行了2条
      -- 因为未知（null）具有特殊的含义，数据库不知道它们是否匹配，
      -- 所以在匹配过滤或不匹配过滤时不返回它们。
   ```

## 第 7 章：过滤数据 —— 组合 WHERE,IN,NOT

1. WHERE
   - AND : 且
   - OR ： 或
   - 计算次序: 括号 > AND > OR
     - WHERE A OR B AND C : B 和 C 的条件，或 A 的条件
     - WHERE (A OR B) AND C : A 或 B 的条件满足后，还有满足 C 的条件
2. IN (A,B,C)
   - IN 操作符完成与 OR 相同的功能
   - 为什么要使用 IN 操作符
     - IN 操作符的语法更清楚且更直观
     - 计算的次序更容易管理
     - IN 操作符一般比 OR 操作符清单执行更快
     - IN 的最大优点是可以包含其他 SELECT 语句
3. NOT
   - NOT A : 否定 A 条件
   - MySQL 支持使用 NOT 对 IN 、BETWEEN 和 EXISTS 子句取反

## 第 8 章：用通配符进行搜索 —— LIKE

1. 名词解释
   - 通配符（ wildcard ） 用来匹配值的一部分的特殊字符。
   - 搜索模式（search pattern）由字面值、通配符或两者组合构成的搜索条件。
   - LIKE 是谓词（predicate）而不是操作符
2. 百分号（%）通配符：%表示任何字符出现任意次数，可以是 1 次，多次，也可以是 0 次
   - LIKE 'jet%' : 查询 jet 开头的词
   - LIKE '%anvil%': 包含 anvil 的值，不论是开头还是结尾
   - LIKE 's%e': 以 s 开头，以 e 结尾
   - LIKE '%' : 匹配所有字符，除了 NULL
   - 注意字段结尾的空格
   - MYSQL 默认是不区分大小写的，如果 LIKE 'jet%'，那么 JetPack 也可以匹配得上
3. 下划线（\_）通配符：匹配单个字符，不能是 0 个
   - LIKE '\_a':
     - .a : 可以匹配
     - a：不可以匹配
     - b.a: 不可以匹配
4. 注意事项
   - 通配符搜索的处理一般要比前面讨论的其他搜索所花时间更长
   - 不要过度使用通配符。如果其他操作符能达到相同的目的，应该使用其他操作符。
   - 在确实需要使用通配符时，除非绝对有必要，否则不要把它们用在搜索模式的开始处。把通配符置于搜索模式的开始处，搜索起来是最慢的。（'%abc'）
   - 仔细注意通配符的位置。如果放错地方，可能不会返回想要的数据

## 第 9 章：用正则表达式进行搜索 —— REGEXP

1. 什么是正则表达式

   - 正则表达式是用来匹配文本的特殊的串（字符集合）
   - MySql 所支持的正则表达式仅为正则表达式语言的一个子集

2. 基本字符匹配

   ```SQL
      mysql> SELECT prod_name FROM products WHERE prod_name REGEXP '1000';
      +--------------+
      | prod_name    |
      +--------------+
      | JetPack 1000 |
      +--------------+
      1 row in set (0.00 sec)
   ```

   REGEXP '1000' 表示匹配 prod_name 中包含 1000 的字段

   ```SQL
      mysql> SELECT prod_name FROM products WHERE prod_name REGEXP '.000';
      +--------------+
      | prod_name    |
      +--------------+
      | JetPack 1000 |
      | JetPack 2000 |
      +--------------+
      2 rows in set (0.00 sec)
   ```

   这里的 `.` 表示：匹配任意**一个**字符

   LIKE 和 REGEXP 的区别：

   - LIKE 是匹配整个字段，如果想匹配部分字段需要使用通配符 `%` 或者 `_`
   - REGEXP 是匹配部分字段，如果想匹配整个字段，需要使用定位符：`^` 和 `$`

   MYSQL 的正则匹配不区分大小写，如果要区分，需要使用关键字 **BINARY**

   ```sql
   mysql> SELECT prod_name FROM products WHERE prod_name REGEXP BINARY  'jet';
   ERROR 3995 (HY000): Character set 'utf8mb4_0900_ai_ci' cannot be used in conjunction with 'binary' in call to regexp_like.
   ```

   **_<font color=red>目前还不知道怎么处理</font>_**

3. 进行 OR 匹配
   ```sql
   mysql> SELECT prod_name FROM products WHERE prod_name REGEXP  '1000|2000';
   +--------------+
   | prod_name    |
   +--------------+
   | JetPack 1000 |
   | JetPack 2000 |
   +--------------+
   2 rows in set (0.00 sec)
   ```
   REGEXP '1000|2000' 匹配包含 1000 或者 2000 的文本
4. 匹配几个字符之一

   ```sql
   -- [123] 表示匹配 ‘1’ ‘2’ ‘3’ 中的任意一个
   -- [123] Ton 相当于 '1 Ton','2 Ton','3 Ton'
   mysql> SELECT prod_name FROM products WHERE prod_name REGEXP  '[123] Ton';
   +-------------+
   | prod_name   |
   +-------------+
   | 1 ton anvil |
   | 2 ton anvil |
   +-------------+
   2 rows in set (0.00 sec)

   -- [123] 是 [1|2|3] 的缩写
   mysql> SELECT prod_name FROM products WHERE prod_name REGEXP  '[1|2|3] Ton';
   +-------------+
   | prod_name   |
   +-------------+
   | 1 ton anvil |
   | 2 ton anvil |
   +-------------+
   2 rows in set (0.00 sec)

   -- 如果不加[]含义会改变：
   -- 1|2|3 Ton 表示 匹配 1 或者 2 或者 3 Ton
   mysql> SELECT prod_name FROM products WHERE prod_name REGEXP  '1|2|3 Ton';
   +---------------+
   | prod_name     |
   +---------------+
   | 1 ton anvil   |
   | 2 ton anvil   |
   | JetPack 1000  |
   | JetPack 2000  |
   | TNT (1 stick) |
   +---------------+
   5 rows in set (0.00 sec)

   -- [^A] 表示否定A集合的范围
   -- [^123] 除了'1','2','3'之外，都可以匹配，如'5','6','7'
   mysql> SELECT prod_name FROM products WHERE prod_name REGEXP  '[^123] Ton';
   +--------------+
   | prod_name    |
   +--------------+
   | .5 ton anvil |
   +--------------+
   1 row in set (0.00 sec)
   ```

5. 匹配范围: `-`
   如`[0-9]`表示数字, `[a-z]` 表示任意小写字母，`[A-Z]` 表示任意大写字母
   ```sql
      mysql> SELECT prod_name FROM products WHERE prod_name REGEXP  '[1-5] Ton';
      +--------------+
      | prod_name    |
      +--------------+
      | .5 ton anvil |
      | 1 ton anvil  |
      | 2 ton anvil  |
      +--------------+
      3 rows in set (0.00 sec)
   ```
6. 匹配特殊字符 `\\`

   `\\.` 查找 `.`

   `\\-` 查找 `-`

   `\\\` 查找 `\`

   `\\f` 表示 换页

   `\\n` 表示 换行

   `\\r` 表示 回车

   `\\t` 表示 制表

   `\\v` 表示 纵向制表

   注意：多数正则表达式实现使用单个反斜杠转义特殊字符，以便能使用这些字符本身。但 MySQL 要求两个反斜杠（MySQL 自己解释一个，正则表达式库解释另一个）。

7. 匹配字符类（character class）
   `[:alnum:]` 任意字母和数字（同[a-zA-Z0-9]）

   `[:alpha:]` 任意字符（同[a-zA-Z]）

   `[:blank:]` 空格和制表（同[\\t]）

   `[:cntrl:]` ASCII 控制字符（ASCII 0 到 31 和 127）

   `[:digit:]` 任意数字（同[0-9]）

   `[:graph:]` 与[:print:]相同，但不包括空格

   `[:lower:]` 任意小写字母（同[a-z]）

   `[:print:]` 任意可打印字符

   `[:punct:]` 既不在[:alnum:]又不在[:cntrl:]中的任意字符

   `[:space:]` 包括空格在内的任意空白字符（同[\\f\\n\\r\\t\\v]）

   `[:upper:]` 任意大写字母（同[A-Z]）

   `[:xdigit:]` 任意十六进制数字（同[a-fA-F0-9]）

8. 匹配多个实例

   `*` 0 个或多个匹配

   `+` 1 个或多个匹配（等于{1,}）

   `?` 0 个或 1 个匹配（等于{0,1}）

   `{n}` 指定数目的匹配

   `{n,}` 不少于指定数目的匹配

   `{n,m}` 匹配数目的范围（m 不超过 255）

   ```sql
   mysql> SELECT prod_name FROM products WHERE prod_name REGEXP  '\\([0-9] sticks?\\)';
   +----------------+
   | prod_name      |
   +----------------+
   | TNT (1 stick)  |
   | TNT (5 sticks) |
   +----------------+
   2 rows in set (0.00 sec)
   ```

   `\\( xxxx \\)` 表示 `()`

   `s?` 匹配 一个 s 或者零个 s，所有 `sticks?` 匹配 sticks 或者 stick

   ```sql
   mysql> SELECT prod_name FROM products WHERE prod_name REGEXP  '[[:digit:]]{4}';
   +--------------+
   | prod_name    |
   +--------------+
   | JetPack 1000 |
   | JetPack 2000 |
   +--------------+
   2 rows in set (0.00 sec)
   ```

   `[[:digit:]]` 表示 `[0-9]`, 也就是`[0-9]{4}`，匹配四个数字

9. 定位符

   `^` 文本的开始

   `$` 文本的结尾

   `[[:<:]]` 词的开始

   `[[:>:]]` 词的结尾

   ```sql
   mysql> SELECT prod_name FROM products WHERE prod_name REGEXP  '^[0-9\\.]';
   +--------------+
   | prod_name    |
   +--------------+
   | .5 ton anvil |
   | 1 ton anvil  |
   | 2 ton anvil  |
   +--------------+
   3 rows in set (0.00 sec)
   ```

   '^[0-9\\.]' 表示以 数字或者`.` 开头。其中 `^` 表示匹配串的开始

   ```sql
   mysql> SELECT prod_name FROM products WHERE prod_name REGEXP  '[[:<:]]se';
   ERROR 3685 (HY000): Illegal argument to a regular expression.
   ```

   **_<font color=red>目前还不知道怎么用</font>_**

10. 简单的正则表达式测试

    ```sql
    mysql> SELECT 'HELLO' REGEXP '[0-9]';
    +------------------------+
    | 'HELLO' REGEXP '[0-9]' |
    +------------------------+
    |                      0 |
    +------------------------+
    1 row in set (0.00 sec)

    mysql> SELECT 'HELLO' REGEXP '[a-z]';
    +------------------------+
    | 'HELLO' REGEXP '[a-z]' |
    +------------------------+
    |                      1 |
    +------------------------+
    1 row in set (0.00 sec)

    mysql> SELECT 'HELLO' REGEXP '[A-Z]';
    +------------------------+
    | 'HELLO' REGEXP '[A-Z]' |
    +------------------------+
    |                      1 |
    +------------------------+
    1 row in set (0.00 sec)
    ```

    - 0 表示不匹配
    - 1 表示匹配

## 第 10 章：创建计算字段

1. 什么是计算字段

   - 不是表中原有的字段
   - 该字段通过原有字段的转换、计算或格式化而得到
   - 计算字段是运行时在 SELECT 语句内创建的
   - 在数据库服务器上完成这些操作比在客户机中完成要快得多，因为 DBMS 是设计来快速有效地完成这种处理的

2. 拼接字段

   ```sql
   mysql> SELECT Concat(RTrim(vend_name), ' (', RTrim(vend_country), ')') AS vend_titile  FROM vendors ORDER BY vend_name;
   +-------------------------+
   | vend_titile             |
   +-------------------------+
   | ACME (USA)              |
   | Anvils R Us (USA)       |
   | Furball Inc. (USA)      |
   | Jet Set (England)       |
   | Jouets Et Ours (France) |
   | LT Supplies (USA)       |
   +-------------------------+
   6 rows in set (0.00 sec)
   ```

   - Concat(A,B,C) 得到 ABC，实现字符串的拼接
   - 去空格
     - RTrim() 去掉串右边的空格
     - LTrim() 去掉串左边的空格
     - Trim() 去掉串左右两边的空格
   - AS 取别名

3. 执行算术计算
   ```sql
   mysql> SELECT prod_id, quantity, item_price, quantity * item_price AS expanded_price FROM orderitems WHERE order_num = 20005;
   +---------+----------+------------+----------------+
   | prod_id | quantity | item_price | expanded_price |
   +---------+----------+------------+----------------+
   | ANV01   |       10 |       5.99 |          59.90 |
   | ANV02   |        3 |       9.99 |          29.97 |
   | TNT2    |        5 |      10.00 |          50.00 |
   | FB      |        1 |      10.00 |          10.00 |
   +---------+----------+------------+----------------+
   4 rows in set (0.01 sec)
   ```
   - `+` 加； `-` 减；`*` 乘；`/` 除
4. 使用 SELECT 测试函数

   ```sql
      mysql> SELECT 3*4;
   +-----+
   | 3*4 |
   +-----+
   |  12 |
   +-----+
   1 row in set (0.00 sec)

   mysql> SELECT Trim('   abc    ');
   +--------------------+
   | Trim('   abc    ') |
   +--------------------+
   | abc                |
   +--------------------+
   1 row in set (0.00 sec)

   mysql> SELECT Now();
   +---------------------+
   | Now()               |
   +---------------------+
   | 2023-06-20 22:45:57 |
   +---------------------+
   1 row in set (0.00 sec)
   ```

## 第 11 章：使用函数处理数据

### 1.文本处理函数

```sql
-- Lower() 将文本转换为小写; Upper()将文本转换为大写
mysql> SELECT Lower('ABCdef'),Upper('ABCdef');
+-----------------+-----------------+
| Lower('ABCdef') | Upper('ABCdef') |
+-----------------+-----------------+
| abcdef          | ABCDEF          |
+-----------------+-----------------+
1 row in set (0.02 sec)

-- RTrim() 去掉串右边的空格; LTrim() 去掉串左边的空格; Trim() 去掉串左右两边的空格

-- Length() 返回串的长度

-- Left() 返回串左边的字符: Left（A,n）返回A字符串最左边n个字符串
mysql> SELECT left('123456',0) ,left('123456',1),left('123456',2);
+------------------+------------------+------------------+
| left('123456',0) | left('123456',1) | left('123456',2) |
+------------------+------------------+------------------+
|                  | 1                | 12               |
+------------------+------------------+------------------+
1 row in set (0.00 sec)

-- Right() 返回串右边的字符：Right（A,n）返回A字符串最右边n个字符串
mysql> SELECT Right('123456',0) ,Right('123456',1),Right('123456',2),Right('123456',4);
+-------------------+-------------------+-------------------+-------------------+
| Right('123456',0) | Right('123456',1) | Right('123456',2) | Right('123456',4) |
+-------------------+-------------------+-------------------+-------------------+
|                   | 6                 | 56                | 3456              |
+-------------------+-------------------+-------------------+-------------------+
1 row in set (0.00 sec)

-- Locate() : Locate(a,A): 找到子串a在字符串A中，第一次出现的位置，从1开始
mysql> SELECT Locate('2','12345'),Locate('34','12345');
+---------------------+----------------------+
| Locate('2','12345') | Locate('34','12345') |
+---------------------+----------------------+
|                   2 |                    3 |
+---------------------+----------------------+
1 row in set (0.00 sec)
-- Locate() : Locate(a,A,n): 从字符串A的第n个位置开始找，找到子串a在字符串A中，第一次出现的位置，从1开始
mysql> SELECT Locate('2','12345',3),Locate('34','12345',3);
+-----------------------+------------------------+
| Locate('2','12345',3) | Locate('34','12345',3) |
+-----------------------+------------------------+
|                     0 |                      3 |
+-----------------------+------------------------+
1 row in set (0.00 sec)

-- SubString()
-- SubString(A,n) 从字符串A的第n个位置开始，截取字符串。开始位是1
mysql> SELECT SubString('abcd',1),SubString('abcd',2);
+---------------------+---------------------+
| SubString('abcd',1) | SubString('abcd',2) |
+---------------------+---------------------+
| abcd                | bcd                 |
+---------------------+---------------------+
1 row in set (0.00 sec)
-- SubString(A,n,m) 从字符串A的第n个位置开始，截取字符串,截取m个，如果n+m > A,就截取剩余的。开始位是1
mysql> SELECT Right('123456',0) ,Right('123456',1),Right('123456',2),Right('123456',4);
+-------------------+-------------------+-------------------+-------------------+
| Right('123456',0) | Right('123456',1) | Right('123456',2) | Right('123456',4) |
+-------------------+-------------------+-------------------+-------------------+
|                   | 6                 | 56                | 3456              |
+-------------------+-------------------+-------------------+-------------------+
1 row in set (0.00 sec)
```

Soundex(): 书中是这么解释：。SOUNDEX 是一个将任何文本串转换为描述其语音表示的字母数字模式的算法。SOUNDEX 考虑了类似的发音字符和音节，使得能对串进行发音比较而不是字母比较。

简单的说：Soundex()是获取字符串发音的编码，可以用来比较两个字符串的发音是否相似

书中举了如下例子：

```sql
   mysql>  SELECT Soundex('Y.Lie' ),Soundex('Y.Lee');
   +-------------------+------------------+
   | Soundex('Y.Lie' ) | Soundex('Y.Lee') |
   +-------------------+------------------+
   | Y400              | Y400             |
   +-------------------+------------------+
   1 row in set (0.00 sec)
```

但我在自己测试的时候遇到如下问题：

第一，中文无法比较

```sql
mysql>  SELECT Soundex('一' ),Soundex('依');
+----------------+---------------+
| Soundex('一' ) | Soundex('依') |
+----------------+---------------+
| 一000          | 依000         |
+----------------+---------------+
1 row in set (0.00 sec)
```

第二，在英语中，两个发音一致的单词，也不一样

```sql
mysql> SELECT Soundex('air' ),Soundex('heir');
+-----------------+-----------------+
| Soundex('air' ) | Soundex('heir') |
+-----------------+-----------------+
| A600            | H600            |
+-----------------+-----------------+
1 row in set (0.00 sec)
```

### 2.日期和时间处理函数

```sql
-- CurDate() 返回当前日期;  CurTime() 返回当前时间;  Now() 返回当前日期和时间
mysql> SELECT CurDate();
+------------+
| CurDate()  |
+------------+
| 2023-06-22 |
+------------+
1 row in set (0.00 sec)

mysql>
mysql> SELECT CurTime();
+-----------+
| CurTime() |
+-----------+
| 10:38:40  |
+-----------+
1 row in set (0.00 sec)

mysql> SELECT Now();
+---------------------+
| Now()               |
+---------------------+
| 2023-06-22 10:38:59 |
+---------------------+
1 row in set (0.00 sec)

-- Date() 返回日期时间的日期部分; Year() 返回一个日期的年份部分;
-- Month() 返回一个日期的月份部分; Day() 返回一个日期的天数部分
mysql> SELECT DATE(Now()),  Year(Now()),Month(Now()),Day(Now());
+-------------+-------------+--------------+------------+
| DATE(Now()) | Year(Now()) | Month(Now()) | Day(Now()) |
+-------------+-------------+--------------+------------+
| 2023-06-22  |        2023 |            6 |         22 |
+-------------+-------------+--------------+------------+
1 row in set (0.00 sec)

-- Time() 返回一个日期时间的时间部分;  Hour() 返回一个时间的小时部分
-- Minute() 返回一个时间的分钟部分; Second() 返回一个时间的秒部分
mysql> SELECT Time(Now()),  Hour(Now()),Minute(Now()),Second(Now());
+-------------+-------------+---------------+---------------+
| Time(Now()) | Hour(Now()) | Minute(Now()) | Second(Now()) |
+-------------+-------------+---------------+---------------+
| 10:49:22    |          10 |            49 |            22 |
+-------------+-------------+---------------+---------------+
1 row in set (0.00 sec)
-- DayOfWeek() 对于一个日期，返回对应的星期几; 从星期天开始（1），到星期六结束（7）
-- DayName() 星期几的名字
mysql> SELECT DayName(Now()),  DayOfWeek(Now());
+----------------+------------------+
| DayName(Now()) | DayOfWeek(Now()) |
+----------------+------------------+
| Thursday       |                5 |
+----------------+------------------+
1 row in set (0.00 sec)

-- Date_Format() 返回一个格式化的日期或时间串
-- 常用：
--       %a 缩写星期名  %W 星期名
--       %b 缩写月名    %c 月，数值    %M 月名  %m 月，数值(00-12)
--       %d 月的天，数值(00-31)  %e 月的天，数值(0-31)
--       %Y 年，4 位    %y 年，2 位
--       %H	小时 (00-23)   %h	小时 (01-12)   %I	小时 (01-12)
--       %i	分钟，数值(00-59)
--       %S	秒(00-59)   %s	秒(00-59)
--       %p	AM 或 PM
mysql> SELECT DATE_FORMAT(NOW(),'%b %d %Y %h:%i %p');
+----------------------------------------+
| DATE_FORMAT(NOW(),'%b %d %Y %h:%i %p') |
+----------------------------------------+
| Jun 22 2023 10:59 AM                   |
+----------------------------------------+
1 row in set (0.00 sec)
-- DATE_ADD(date,INTERVAL expr type)
--       date 参数是合法的日期表达式。expr 参数是您希望添加的时间间隔。
--       type 参数可以是下列值：YEAR、MONTH、DAY、HOUR、MINUTE、SECOND、WEEK...
mysql> SELECT DATE_ADD(Now(),INTERVAL 1 YEAR);
+---------------------------------+
| DATE_ADD(Now(),INTERVAL 1 YEAR) |
+---------------------------------+
| 2024-06-22 11:13:20             |
+---------------------------------+
1 row in set (0.00 sec)
-- AddDate()默认增加1天，AddTime()默认增加1秒
mysql> SELECT NOW(), ADDDATE(Now(), 1 ),ADDTIME(Now(), 1 );
+---------------------+---------------------+---------------------+
| NOW()               | ADDDATE(Now(), 1 )  | ADDTIME(Now(), 1 )  |
+---------------------+---------------------+---------------------+
| 2023-06-22 11:16:16 | 2023-06-23 11:16:16 | 2023-06-22 11:16:17 |
+---------------------+---------------------+---------------------+
1 row in set (0.00 sec)
-- AddDate()默认增加1天，其他和DATE_ADD()类似
mysql> SELECT NOW(), ADDDATE(Now(),INTERVAL 1 MONTH );
+---------------------+----------------------------------+
| NOW()               | ADDDATE(Now(),INTERVAL 1 MONTH ) |
+---------------------+----------------------------------+
| 2023-06-22 11:26:33 | 2023-07-22 11:26:33              |
+---------------------+----------------------------------+
1 row in set (0.00 sec)
-- AddTime() 第二个参数是一个表达式，把相应的加起来
mysql> SELECT Now(), ADDTIME(Now(), '1:2:1' );
+---------------------+--------------------------+
| Now()               | ADDTIME(Now(), '1:2:1' ) |
+---------------------+--------------------------+
| 2023-06-22 11:35:40 | 2023-06-22 12:37:41      |
+---------------------+--------------------------+
1 row in set (0.00 sec)
-- 用 负数表示减
mysql> SELECT Now(), ADDTIME(Now(), -1 ),ADDDATE(Now(), -1 );
+---------------------+---------------------+---------------------+
| Now()               | ADDTIME(Now(), -1 ) | ADDDATE(Now(), -1 ) |
+---------------------+---------------------+---------------------+
| 2023-06-22 11:43:45 | 2023-06-22 11:43:44 | 2023-06-21 11:43:45 |
+---------------------+---------------------+---------------------+
1 row in set (0.00 sec)
-- DATE_SUB() 表示减
mysql> SELECT Now(), DATE_SUB(Now(),INTERVAL 1 YEAR);
+---------------------+---------------------------------+
| Now()               | DATE_SUB(Now(),INTERVAL 1 YEAR) |
+---------------------+---------------------------------+
| 2023-06-22 11:44:39 | 2022-06-22 11:44:39             |
+---------------------+---------------------------------+
1 row in set (0.00 sec)

-- DateDiff(A,B) 计算两个日期之差(A-B)
mysql> SELECT DateDiff(NOW(),'2024-01-01');
+------------------------------+
| DateDiff(NOW(),'2024-01-01') |
+------------------------------+
|                         -193 |
+------------------------------+
1 row in set (0.00 sec)
```

### 3.数值处理函数: 很少用

Abs() 返回一个数的绝对值

Cos() 返回一个角度的余弦

Exp() 返回一个数的指数值

Mod() 返回除操作的余数

Pi() 返回圆周率

Rand() 返回一个随机数

Sin() 返回一个角度的正弦

Sqrt() 返回一个数的平方根

Tan() 返回一个角度的正切

## 第 12 章：汇总数据

1. 聚集函数

   - AVG() 返回某列的平均值

     - 只用于单个列
     - AVG()函数忽略列值为 NULL 的行

   - COUNT() 返回某列的行数

     - 使用 COUNT(\*)对表中行的数目进行计数，不管表列中包含的是空值（NULL）还是非空值。
     - 使用 COUNT(column)对特定列中具有值的行进行计数，忽略 NULL 值。

   - MAX() 返回某列的最大值

     - MAX()函数忽略列值为 NULL 的行。
     - MAX()也可以用于文本列，

   - MIN() 返回某列的最小值

     - MIN()函数忽略列值为 NULL 的行。
     - MIN()也可以用于文本列，

   - SUM() 返回某列值之和
     - SUM()函数忽略列值为 NULL 的行。

   ```sql
   -- 所有聚集函数都可用来执行多个列上的计算。
   mysql> SELECT SUM(item_price * quantity)As total_price FROM orderitems ;
   +-------------+
   | total_price |
   +-------------+
   |     1368.34 |
   +-------------+
   1 row in set (0.00 sec)
   ```

   - DISTINCT() 去重

     ```sql
     -- DISTINCT（）函数和 DISTINCT关键字的去重效果是一样的
     -- 不去重前有7个
     mysql> SELECT prod_price,prod_id FROM products WHERE vend_id = 1003;
     +------------+---------+
     | prod_price | prod_id |
     +------------+---------+
     |      13.00 | DTNTR   |
     |      10.00 | FB      |
     |       2.50 | FC      |
     |      50.00 | SAFE    |
     |       4.49 | SLING   |
     |       2.50 | TNT1    |
     |      10.00 | TNT2    |
     +------------+---------+
     7 rows in set (0.00 sec)

     -- 对prod_price去重后还有7个，应该是prod_id影响了
     mysql> SELECT DISTINCT(prod_price),prod_id FROM products WHERE vend_id = 1003;
     +------------+---------+
     | prod_price | prod_id |
     +------------+---------+
     |      13.00 | DTNTR   |
     |      10.00 | FB      |
     |       2.50 | FC      |
     |      50.00 | SAFE    |
     |       4.49 | SLING   |
     |       2.50 | TNT1    |
     |      10.00 | TNT2    |
     +------------+---------+
     7 rows in set (0.00 sec)

     -- 去掉 prod_id 后，再去重，变成4条
     mysql> SELECT DISTINCT(prod_price) FROM products WHERE vend_id = 1003;
     +------------+
     | prod_price |
     +------------+
     |      13.00 |
     |      10.00 |
     |       2.50 |
     |      50.00 |
     |       4.49 |
     +------------+
     5 rows in set (0.00 sec)

     -- 可以和其他聚合函数一起使用
     mysql> SELECT AVG( DISTINCT(prod_price)) FROM products WHERE vend_id = 1003;
     +----------------------------+
     | AVG( DISTINCT(prod_price)) |
     +----------------------------+
     |                  15.998000 |
     +----------------------------+
     1 row in set (0.00 sec)
     ```

## 第 13 章：分组数据

1. 创建分组 GROUP BY
   ```sql
   mysql> SELECT vend_id, count(*) FROM products  GROUP BY vend_id ;
   +---------+----------+
   | vend_id | count(*) |
   +---------+----------+
   |    1001 |        3 |
   |    1002 |        2 |
   |    1003 |        7 |
   |    1005 |        2 |
   +---------+----------+
   4 rows in set (0.00 sec)
   ```
   - 形式为 SELECT a,b, 聚合函数 FROM 表 GROUP BY a,b
     - 除了聚合函数和被 GROUP BY 的 a/b,不能 SELECT 其他列
   - 如果分组列中具有 NULL 值，则 NULL 将作为一个分组返回。如果列中有多行 NULL 值，它们将分为一组。
   - SELECT ... FROM ... WHERE...GROUP BY ... ORDER BY ...
2. 使用 WITH ROLLUP 关键字
   ```sql
   -- WITH ROLLUP 的作用是对每个分组的结果进行统计
   mysql> SELECT vend_id, count(*) FROM products  GROUP BY vend_id WITH ROLLUP;
   +---------+----------+
   | vend_id | count(*) |
   +---------+----------+
   |    1001 |        3 |
   |    1002 |        2 |
   |    1003 |        7 |
   |    1005 |        2 |
   |    NULL |       14 |
   +---------+----------+
   5 rows in set (0.00 sec)
   ```
3. 可以根据多个字段进行分组

   GROUP BY A,B : 先根据 A 分组，再根据 B 分组

   ```sql
   mysql> SELECT vend_id,prod_price, count(*) FROM products  GROUP BY vend_id,prod_price WITH ROLLUP;
   +---------+------------+----------+
   | vend_id | prod_price | count(*) |
   +---------+------------+----------+
   |    1001 |       5.99 |        1 |
   |    1001 |       9.99 |        1 |
   |    1001 |      14.99 |        1 |
   |    1001 |       NULL |        3 |
   |    1002 |       3.42 |        1 |
   |    1002 |       8.99 |        1 |
   |    1002 |       NULL |        2 |
   |    1003 |       2.50 |        2 |
   |    1003 |       4.49 |        1 |
   |    1003 |      10.00 |        2 |
   |    1003 |      13.00 |        1 |
   |    1003 |      50.00 |        1 |
   |    1003 |       NULL |        7 |
   |    1005 |      35.00 |        1 |
   |    1005 |      55.00 |        1 |
   |    1005 |       NULL |        2 |
   |    NULL |       NULL |       14 |
   +---------+------------+----------+
   17 rows in set (0.00 sec)
   ```

4. 过滤分组 HAVING： 起到 where 的作用

   ```sql
   mysql> SELECT prod_price ,vend_id, count(*) FROM products  GROUP BY prod_price,vend_id WITH ROLLUP HAVING prod_price < 10 ;
   +------------+---------+----------+
   | prod_price | vend_id | count(*) |
   +------------+---------+----------+
   |       2.50 |    1003 |        2 |
   |       2.50 |    NULL |        2 |
   |       3.42 |    1002 |        1 |
   |       3.42 |    NULL |        1 |
   |       4.49 |    1003 |        1 |
   |       4.49 |    NULL |        1 |
   |       5.99 |    1001 |        1 |
   |       5.99 |    NULL |        1 |
   |       8.99 |    1002 |        1 |
   |       8.99 |    NULL |        1 |
   |       9.99 |    1001 |        1 |
   |       9.99 |    NULL |        1 |
   +------------+---------+----------+
   12 rows in set (0.00 sec)

   mysql> SELECT prod_price ,vend_id, count(*) FROM products  GROUP BY prod_price,vend_id WITH ROLLUP HAVING count(*) >1 ;
   +------------+---------+----------+
   | prod_price | vend_id | count(*) |
   +------------+---------+----------+
   |       2.50 |    1003 |        2 |
   |       2.50 |    NULL |        2 |
   |      10.00 |    1003 |        2 |
   |      10.00 |    NULL |        2 |
   |       NULL |    NULL |       14 |
   +------------+---------+----------+
   5 rows in set (0.00 sec)
   ```

   - 首先 HAVING 是在 GROUP BY 之后才执行过滤
   - HAVING 会 忽视 null 值
   - SELECT ... FROM ... WHERE...GROUP BY ... HAVING... ORDER BY ... LIMIT...

## 第 14 章：使用子查询

1. 什么是子查询：嵌套在其他查询中的查询
2. 利用子查询进行过滤
   ```sql
   mysql> SELECT cust_name,cust_contact FROM customers
         WHERE cust_id IN
            (SELECT cust_id FROM orders WHERE order_num IN
                  (SELECT order_num FROM orderitems WHERE prod_id = 'TNT2'));
   +----------------+--------------+
   | cust_name      | cust_contact |
   +----------------+--------------+
   | Coyote Inc.    | Y Lee        |
   | Yosemite Place | Y Sam        |
   +----------------+--------------+
   2 rows in set (0.01 sec)
   ```
3. 作为计算字段使用子查询
   ```sql
   mysql> SELECT   cust_name,
      ->          cust_state,
      ->          (SELECT COUNT(*) FROM orders
      ->             WHERE orders.cust_id = customers.cust_id) AS orders
      -> FROM customers
      -> ORDER BY cust_name;
   +----------------+------------+--------+
   | cust_name      | cust_state | orders |
   +----------------+------------+--------+
   | Coyote Inc.    | MI         |      2 |
   | E Fudd         | IL         |      1 |
   | Mouse House    | OH         |      0 |
   | Wascals        | IN         |      1 |
   | Yosemite Place | AZ         |      1 |
   +----------------+------------+--------+
   5 rows in set (0.00 sec)
   ```
   - 该子查询执行了 5 次，因为检索出了 5 个客户
4. 子查询有性能问题

## 第 15 章：联结表

1. 使用 WHERE 子句定义联结
   ```sql
   mysql> SELECT cust_name,cust_contact
      -> FROM customers,orders,orderitems
      -> WHERE customers.cust_id = orders.cust_id
      -> AND orderitems.order_num = orders.order_num
      -> AND prod_id = 'TNT2' ;
   +----------------+--------------+
   | cust_name      | cust_contact |
   +----------------+--------------+
   | Coyote Inc.    | Y Lee        |
   | Yosemite Place | Y Sam        |
   +----------------+--------------+
   2 rows in set (0.00 sec)
   ```
   - 该 sql 先是把三张表的每条数据做一一匹配，即 customers*orders*orderitems 条数据（笛卡尔积）
   - 接着再根据 WHERE 子句进行过滤
   - 这种处理非常消耗新能，因为如果没张表有 1000 条数据，那么三张表联结就会有 1000*1000*1000=10 亿条数据
2. INNER JOIN 语法
   ```sql
   mysql> SELECT cust_name,cust_contact
      -> FROM customers
      -> INNER JOIN orders ON customers.cust_id = orders.cust_id
      -> INNER JOIN orderitems ON orderitems.order_num = orders.order_num
      -> WHERE  prod_id = 'TNT2' ;
   +----------------+--------------+
   | cust_name      | cust_contact |
   +----------------+--------------+
   | Coyote Inc.    | Y Lee        |
   | Yosemite Place | Y Sam        |
   +----------------+--------------+
   2 rows in set (0.00 sec)
   ```
3. 上述两种写法的区别
   在《MySql 必知必会》这本书中，仅说明这是语法的差别，并给出如下建议：

   ```txt
   使用哪种语法: ANSI SQL规范首选INNER JOIN语法。此外，尽管使用WHERE子句定义联结的确比较简单，
   但是使用明确的联结语法能够确保不会忘记联结条件，有时候这样做也能影响性能
   ```

   但我在网上看到了如下的[观点](https://blog.csdn.net/fengzyf/article/details/100080852)：

   ```txt
      where条件是在临时表生成好后，再对临时表进行过滤的条件。
      所以如果有两个表分别有10000条记录，一起做联结查询，就会有1亿条记录，接着再执行where，就需要从1亿条记录里面找出符合联结条件的数据
      这样会大大影响效率

      on条件是在生成临时表时使用的条件。所以在生成临时表的时候就已经筛选好了。认为这样会更有效率
   ```

   但我实际测试，发现是一样快的, 而且效率有很高

   ```sql
   select SQL_NO_CACHE * from test1 inner join test2 on test1.id=test2.link_id inner join test3 on test2.id=test3.link_id ;
   500 rows in set, 1 warning (0.00 sec)

   select SQL_NO_CACHE * from test1,test2,test3 where test1.id=test2.link_id and test2.id=test3.link_id;
   500 rows in set, 1 warning (0.00 sec)
   ```

   所以我觉得 `where test1.id=test2.link_id and test2.id=test3.link_id` 与 ` on test1.id=test2.link_id`,`on test2.id=test3.link_id` 起到的效果是一样的

   另外也我也发现以下情况：

   ```sql
   -- 当我去掉一个条件时，查询时间明显变长
   select SQL_NO_CACHE * from test1,test2,test3 where test1.id=test2.link_id;
   250000 rows in set, 1 warning (0.21 sec)

   select SQL_NO_CACHE * from test1 inner join test2 on test1.id=test2.link_id inner join test3
   250000 rows in set, 1 warning (0.21 sec)
   ```

   **_<font color=red>以上这些疑问需要后期继续深入学习才可以解开</font>_**

## 第 16 章：创建高级联结

1. 内部联结（等值联结）: inner join / FROM tableA,tableB
2. 自联结: 自己联结自己

   ```sql
   -- 背景：假如你发现某物品（其ID为DTNTR）存在问题，因此想知道生产该物品的供应商生产的其他物品是否也存在这些问题
   -- 使用子查询
   mysql> SELECT prod_id, prod_name FROM products WHERE vend_id =
      ->    (SELECT vend_id FROM products WHERE prod_id = 'DTNTR') ;
      +---------+----------------+
      | prod_id | prod_name      |
      +---------+----------------+
      | DTNTR   | Detonator      |
      | FB      | Bird seed      |
      | FC      | Carrots        |
      | SAFE    | Safe           |
      | SLING   | Sling          |
      | TNT1    | TNT (1 stick)  |
      | TNT2    | TNT (5 sticks) |
      +---------+----------------+
      7 rows in set (0.02 sec)
   -- 使用自联结
   mysql> SELECT p1.prod_id, p1.prod_name
      -> FROM products AS p1,products AS p2
      -> WHERE p1.vend_id = p2.vend_id
      -> AND p2.prod_id = 'DTNTR';
      +---------+----------------+
      | prod_id | prod_name      |
      +---------+----------------+
      | DTNTR   | Detonator      |
      | FB      | Bird seed      |
      | FC      | Carrots        |
      | SAFE    | Safe           |
      | SLING   | Sling          |
      | TNT1    | TNT (1 stick)  |
      | TNT2    | TNT (5 sticks) |
      +---------+----------------+
      7 rows in set (0.00 sec)
   -- 使用自联结必须用别名：因为单条SELECT语句中不能引用两次相同的表
   -- 作者建议：用自联结而不用子查询，因为效率更高
   ```

3. 自然联结（书中定义）

   - 少有一个列出现在不止一个表中（被联结的列）（存在联结条件）
   - 自然联结排除多次出现，使每个列只返回一次（SELECT 的结果不重复）
   - 内部联结都是自然联结
   - 在[网上](https://blog.csdn.net/tanglincsdn/article/details/46274351)看到如下定义：
     自然连接(Natural join)是一种特殊的等值连接，要求两个关系表中进行比较的属性组必须是名称相同的属性组，并且在结果中把重复的属性列去掉（即：留下名称相同的属性组中的其中一组）。

   ```sql
   SELECT c.*, o.order_num, o.order_date,oi.prod_id,oi.quantity,oi.item_price
   FROM customers AS c, orders AS o, orderitems AS oi
   WHERE c.cust_id = o.cust_id
   AND oi.order_num = o.order_num
   AND prod_id = 'FB' ;

   -- 如果是两张表还可以用 natrual  join
   SELECT * FROM customers  natrual  join orders  ;
   ```

4. 外部联结

   - 外部联结与内联结的区别：外连接不仅返回匹配的行，也会返回不匹配的行
   - LEFT OUTER JOIN：左联结，OUTER 可省略
   - RIGHT OUTER JOIN：右联结，OUTER 可省略

   ```sql
   -- 内联结
   mysql> SELECT customers.cust_id, orders.order_num
      -> FROM customers INNER JOIN orders
      -> ON customers.cust_id = orders.cust_id;
   +---------+-----------+
   | cust_id | order_num |
   +---------+-----------+
   |   10001 |     20005 |
   |   10001 |     20009 |
   |   10003 |     20006 |
   |   10004 |     20007 |
   |   10005 |     20008 |
   +---------+-----------+
   5 rows in set (0.00 sec)

   -- 左联结
   mysql> SELECT customers.cust_id, orders.order_num
      -> FROM customers LEFT OUTER JOIN orders
      -> ON customers.cust_id = orders.cust_id;
   +---------+-----------+
   | cust_id | order_num |
   +---------+-----------+
   |   10001 |     20005 |
   |   10001 |     20009 |
   |   10002 |      NULL |
   |   10003 |     20006 |
   |   10004 |     20007 |
   |   10005 |     20008 |
   +---------+-----------+
   6 rows in set (0.00 sec)

   -- 右联结
   mysql> SELECT orders.order_num,customers.cust_id
      -> FROM orders RIGHT OUTER JOIN customers
      -> ON customers.cust_id = orders.cust_id;
   +-----------+---------+
   | order_num | cust_id |
   +-----------+---------+
   |     20005 |   10001 |
   |     20009 |   10001 |
   |      NULL |   10002 |
   |     20006 |   10003 |
   |     20007 |   10004 |
   |     20008 |   10005 |
   +-----------+---------+
   6 rows in set (0.00 sec)
   ```

5. 使用带聚集函数的联结

   ```sql
   -- 检索所有客户及每个客户所下的订单数
   mysql> SELECT customers.cust_name,customers.cust_id, COUNT(orders.order_num) AS num_ord
      -> FROM customers INNER JOIN orders
      -> ON customers.cust_id = orders.cust_id
      -> GROUP BY customers.cust_id;
   +----------------+---------+---------+
   | cust_name      | cust_id | num_ord |
   +----------------+---------+---------+
   | Coyote Inc.    |   10001 |       2 |
   | Wascals        |   10003 |       1 |
   | Yosemite Place |   10004 |       1 |
   | E Fudd         |   10005 |       1 |
   +----------------+---------+---------+
   4 rows in set (0.00 sec)
   ```

   ```txt
   总结： table A 和 table B
   A INNER JOIN B : 取 A和B的交集
   A LEFT OUTER JOIN B : 取 A
   A RIGHT OUTER JOIN B : 取 B
   ```

## 第 17 章：组合查询 UNION

```sql
mysql> SELECT vend_id,prod_id,prod_price
    -> FROM products
    -> WHERE prod_price <= 5
    -> UNION
    -> SELECT vend_id,prod_id,prod_price
    -> FROM products
    -> WHERE vend_id IN (1001,1002);
   +---------+---------+------------+
   | vend_id | prod_id | prod_price |
   +---------+---------+------------+
   |    1003 | FC      |       2.50 |
   |    1002 | FU1     |       3.42 |
   |    1003 | SLING   |       4.49 |
   |    1003 | TNT1    |       2.50 |
   |    1001 | ANV01   |       5.99 |
   |    1001 | ANV02   |       9.99 |
   |    1001 | ANV03   |      14.99 |
   |    1002 | OL1     |       8.99 |
   +---------+---------+------------+
   8 rows in set (0.00 sec)

-- UNION查询相同的表相当于使用多个WHERE条件
-- SELECT vend_id,prod_id,prod_price FROM products WHERE prod_price<=5 OR vend_id IN (1001,1002);

-- UNION必须由两条或两条以上的SELECT语句组成

-- UNION中的每个查询必须包含相同的列、表达式或聚集函数

-- 列数据类型必须兼容

-- UNION是去重的，用UNION ALL 是不去重的

-- ORDER BY要放在最后一个SELECT后面，而且只能用一个ORDER BY
mysql> SELECT vend_id,prod_id,prod_price
    -> FROM products
    -> WHERE prod_price <= 5
    -> UNION ALL
    -> SELECT vend_id,prod_id,prod_price
    -> FROM products
    -> WHERE vend_id IN (1001,1002)
    -> ORDER BY vend_id, prod_price;
```

## 第 18 章：全文本搜索 Match() Against()

### 1. 如何理解全文本搜索

1. 为什么要使用全文本搜索

   - 模糊查询（LIKE,REGEXP）有许多缺点
     - 性能问题: 会尝试匹配所有行，通常也没索引
     - 无法明确控制那些匹配哪些不匹配
     - 不智能，只会把所有匹配结果都搜出来，无法智能排序，也无法剔除一部分结果
   - 全文搜索可以解决上述问题：在使用全文本搜索时，MySQL 不需要分别查看每个行，不需要分别分析和处理每个词。
     **MySQL 创建指定列中各词的一个索引**，搜索可以针对这些词进行。
     这样，MySQL 可以快速有效地决定哪些词匹配（哪些行包含它们），哪些词不匹配，它们匹配的频率，等等。

   普通索引是针对每列做索引，全文搜索的索引是根据每列里面的每个词作索引。

2. 并非所有引擎都支持全文本搜索
   - InnoDB 不支持
   - MyISAM 支持

### 2. 如何使用全文搜索

1. 启用全文本搜索支持

   - 在建表的时候指定搜索引擎:ENGINE=MyISAM
   - 同时设置要全文搜索的列：PRIMARY KEY(note_id), 会对 note_id 做索引
     ```sql
     CREATE TABLE productnotes
     (
     note_id    int           NOT NULL AUTO_INCREMENT,
     prod_id    char(10)      NOT NULL,
     note_date datetime       NOT NULL,
     note_text  text          NULL ,
     PRIMARY KEY(note_id),
     FULLTEXT(note_text)
     ) ENGINE=MyISAM;
     ```
   - 也可以在建表之后指定 FULLTEXT
   - 如果建表后要导入数据，就不要再建表时用 FULLTEXT，因为更新索引要花时间
   - FULLTEXT 可以索引单列也可以索引多列

2. 进行全文本搜索

   ```sql
   mysql> SELECT note_id, note_text FROM productnotes WHERE Match(note_text) Against('rabbit');
   +---------+-----------------------------------------------------------------------------------------------------------------------+
   | note_id | note_text                                                                                                             |
   +---------+-----------------------------------------------------------------------------------------------------------------------+
   |     110 | Customer complaint: rabbit has been able to detect trap, food apparently less effective now.                          |
   |     104 | Quantity varies, sold by the sack load.All guaranteed to be bright and orange, and suitable for use as rabbit bait.   |
   +---------+-----------------------------------------------------------------------------------------------------------------------+
   2 rows in set (0.00 sec)

   mysql> SELECT note_id, note_text FROM productnotes WHERE note_text LIKE '%rabbit%' ;
   +---------+-----------------------------------------------------------------------------------------------------------------------+
   | note_id | note_text                                                                                                             |
   +---------+-----------------------------------------------------------------------------------------------------------------------+
   |     104 | Quantity varies, sold by the sack load.All guaranteed to be bright and orange, and suitable for use as rabbit bait.   |
   |     110 | Customer complaint: rabbit has been able to detect trap, food apparently less effective now.                          |
   +---------+-----------------------------------------------------------------------------------------------------------------------+
   2 rows in set (0.00 sec)
   ```

   - Against()指定搜索文本
   - 传递给 Match() 的值必须与 FULLTEXT()定义中的相同。如果指定多个列，则必须列出它们，而且次序正确。
   - 搜索不区分大小写
   - 全文搜索和 LIKE 搜索的顺序不一样，是因为全文搜索根据算法进行了排序

   ```sql
   mysql> SELECT note_id, note_text, Match(note_text) Against('rabbit') AS ranks FROM productnotes;
   +---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------+
   | note_id | note_text                                                                                                                                                | ranks              |
   +---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------+
   |     101 | Customer complaint:Sticks not individually wrapped, too easy to mistakenly detonate all at once.Recommend individual wrapping.                           |                  0 |
   |     102 | Can shipped full, refills not available.Need to order new can if refill needed.                                                                          |                  0 |
   |     103 | Safe is combination locked, combination not provided with safe.This is rarely a problem as safes are typically blown up or dropped by customers.         |                  0 |
   |     104 | Quantity varies, sold by the sack load.All guaranteed to be bright and orange, and suitable for use as rabbit bait.                                      | 1.5905543565750122 |
   |     105 | Included fuses are short and have been known to detonate too quickly for some customers.Longer fuses are available (item FU1) and should be recommended. |                  0 |
   |     106 | Matches not included, recommend purchase of matches or detonator (item DTNTR).                                                                           |                  0 |
   |     107 | Please note that no returns will be accepted if safe opened using explosives.                                                                            |                  0 |
   |     108 | Multiple customer returns, anvils failing to drop fast enough or falling backwards on purchaser. Recommend that customer considers using heavier anvils. |                  0 |
   |     109 | Item is extremely heavy. Designed for dropping, not recommended for use with slings, ropes, pulleys, or tightropes.                                      |                  0 |
   |     110 | Customer complaint: rabbit has been able to detect trap, food apparently less effective now.                                                             | 1.6408053636550903 |
   |     111 | Shipped unassembled, requires common tools (including oversized hammer).                                                                                 |                  0 |
   |     112 | Customer complaint:Circular hole in safe floor can apparently be easily cut with handsaw.                                                                |                  0 |
   |     113 | Customer complaint:Not heavy enough to generate flying stars around head of victim. If being purchased for dropping, recommend ANV02 or ANV03 instead.   |                  0 |
   |     114 | Call from individual trapped in safe plummeting to the ground, suggests an escape hatch be added.Comment forwarded to vendor.                            |                  0 |
   +---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------+
   14 rows in set (0.00 sec)
   ```

   - Match()和 Against()用来建立一个计算列（别名为 ranks），此列包含全文本搜索计算出的等级值
   - where 会排除等级为 0 的行，然后根据等级降序排列
   - 等级由 MySQL 根据行中词的数目、唯一词的数目、整个索引中词的总数以及包含该词的行的数目计算出来
   - 在这个例子中：110 的 rabbit 出现的更早，所以等级会更高

### 3.使用查询扩展 WITH QUERY EXPANSION

```sql
mysql> SELECT note_id, note_text FROM productnotes WHERE Match(note_text) Against('anvils');
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
| note_id | note_text                                                                                                                                                |
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
|     108 | Multiple customer returns, anvils failing to drop fast enough or falling backwards on purchaser. Recommend that customer considers using heavier anvils. |
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)

mysql> SELECT note_id, note_text FROM productnotes WHERE Match(note_text) Against('anvils' WITH QUERY EXPANSION);
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
| note_id | note_text                                                                                                                                                |
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
|     108 | Multiple customer returns, anvils failing to drop fast enough or falling backwards on purchaser. Recommend that customer considers using heavier anvils. |
|     101 | Customer complaint:Sticks not individually wrapped, too easy to mistakenly detonate all at once.Recommend individual wrapping.                           |
|     113 | Customer complaint:Not heavy enough to generate flying stars around head of victim. If being purchased for dropping, recommend ANV02 or ANV03 instead.   |
|     107 | Please note that no returns will be accepted if safe opened using explosives.                                                                            |
|     110 | Customer complaint: rabbit has been able to detect trap, food apparently less effective now.                                                             |
|     112 | Customer complaint:Circular hole in safe floor can apparently be easily cut with handsaw.                                                                |
|     106 | Matches not included, recommend purchase of matches or detonator (item DTNTR).                                                                           |
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
7 rows in set (0.00 sec)
```

- 查询扩展会把与搜索词相关的词也查出来
- 首先，进行一个基本的全文本搜索，找出与搜索条件匹配的所有行；
- 其次，MySQL 检查这些匹配行并选择所有有用的词（作者没有详细解释什么是有用的词）
- 再其次，MySQL 再次进行全文本搜索，这次不仅使用原来的条件，而且还使用所有有用的词。

### 4.布尔文本搜索 IN BOOLEAN MODE

- 全文本布尔操作符

  - `+` 包含，词必须存在
  - `-` 排除，词必须不出现
  - `>` 包含，而且增加等级值
  - `<` 包含，且减少等级值
  - `()` 把词组成子表达式（允许这些子表达式作为一个组被包含、排除、排列等）
  - `~` 取消一个词的排序值
  - `*` 词尾的通配符
  - `""` 定义一个短语（与单个词的列表不一样，它匹配整个短语以便包含或排除这个短语）

- 案例

```sql
-- 排除以rope开头的单词
mysql> SELECT note_id, note_text FROM productnotes WHERE Match(note_text) Against('heavy' IN BOOLEAN MODE );
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
| note_id | note_text                                                                                                                                                |
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
|     109 | Item is extremely heavy. Designed for dropping, not recommended for use with slings, ropes, pulleys, or tightropes.                                      |
|     113 | Customer complaint:Not heavy enough to generate flying stars around head of victim. If being purchased for dropping, recommend ANV02 or ANV03 instead.   |
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
2 rows in set (0.00 sec)

mysql> SELECT note_id, note_text FROM productnotes WHERE Match(note_text) Against('heavy -rope*' IN BOOLEAN MODE );
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
| note_id | note_text                                                                                                                                                |
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
|     113 | Customer complaint:Not heavy enough to generate flying stars around head of victim. If being purchased for dropping, recommend ANV02 or ANV03 instead.   |
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

```sql
-- +A +B : A和B都要包含
mysql> SELECT note_id, note_text FROM productnotes WHERE Match(note_text) Against('+shipped' IN BOOLEAN MODE );
+---------+-----------------------------------------------------------------------------------+
| note_id | note_text                                                                         |
+---------+-----------------------------------------------------------------------------------+
|     102 | Can shipped full, refills not available.
Need to order new can if refill needed. |
|     111 | Shipped unassembled, requires common tools (including oversized hammer).          |
+---------+-----------------------------------------------------------------------------------+
2 rows in set (0.00 sec)

mysql> SELECT note_id, note_text FROM productnotes WHERE Match(note_text) Against('+wrapping +shipped' IN BOOLEAN MODE );
Empty set (0.00 sec)
```

```sql
-- "rabbit customers" 就变成搜索rabbit customers这个词组；而'rabbit customers'表示搜索rabbit这个单词，或者customers这个单词
mysql> SELECT note_id, note_text FROM productnotes WHERE Match(note_text) Against('rabbit customers' IN BOOLEAN MODE );
+---------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
| note_id | note_text                                                                                                                                                  |
+---------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
|     103 | Safe is combination locked, combination not provided with safe.This is rarely a problem as safes are typically blown up or dropped by customers.           |
|     104 | Quantity varies, sold by the sack load.All guaranteed to be bright and orange, and suitable for use as rabbit bait.                                        |
|     105 | Included fuses are short and have been known to detonate too quickly for some customers.Longer fuses are available (item FU1) and should be recommended.   |
|     110 | Customer complaint: rabbit has been able to detect trap, food apparently less effective now.                                                               |
+---------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
4 rows in set (0.00 sec)

mysql> SELECT note_id, note_text FROM productnotes WHERE Match(note_text) Against('"rabbit customers"' IN BOOLEAN MODE );
Empty set (0.00 sec)
```

```sql
-- >rabbit：如果查询到这个单词，就给这行增加等级；  <customers：如果查询到这个单词，就给这行减少等级
-- 在布尔方式中，不按等级值降序排序返回数据
mysql> SELECT note_text,Match(note_text) Against('rabbit customers' IN BOOLEAN MODE ) AS ranks FROM productnotes WHERE Match(note_text) Against(' rabbit customers' IN BOOLEAN MODE );
+----------------------------------------------------------------------------------------------------------------------------------------------------------+-------+
| note_text                                                                                                                                                | ranks |
+----------------------------------------------------------------------------------------------------------------------------------------------------------+-------+
| Safe is combination locked, combination not provided with safe.This is rarely a problem as safes are typically blown up or dropped by customers.         |     1 |
| Quantity varies, sold by the sack load.All guaranteed to be bright and orange, and suitable for use as rabbit bait.                                      |     1 |
| Included fuses are short and have been known to detonate too quickly for some customers.Longer fuses are available (item FU1) and should be recommended. |     1 |
| Customer complaint: rabbit has been able to detect trap, food apparently less effective now.                                                             |     1 |
+----------------------------------------------------------------------------------------------------------------------------------------------------------+-------+
4 rows in set (0.00 sec)

mysql> SELECT note_text,Match(note_text) Against('>rabbit <customers' IN BOOLEAN MODE ) AS ranks FROM productnotes WHERE Match(note_text) Against('>rabbit <customers' IN BOOLEAN MODE );
+----------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------+
| note_text                                                                                                                                                | ranks              |
+----------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------+
| Safe is combination locked, combination not provided with safe.This is rarely a problem as safes are typically blown up or dropped by customers.         | 0.6666666865348816 |
| Quantity varies, sold by the sack load.All guaranteed to be bright and orange, and suitable for use as rabbit bait.                                      |                1.5 |
| Included fuses are short and have been known to detonate too quickly for some customers.Longer fuses are available (item FU1) and should be recommended. | 0.6666666865348816 |
| Customer complaint: rabbit has been able to detect trap, food apparently less effective now.                                                             |                1.5 |
+----------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------+
4 rows in set (0.00 sec)
```

```sql
-- +(<combination): ()使 + 和< 共同作用在 combination上
mysql> SELECT  note_text,Match(note_text) Against('+safe +(<combination)' IN BOOLEAN MODE ) AS ranks FROM productnotes WHERE Match(note_text) Against('+safe +(<combination)' IN BOOLEAN MODE );
+----------------------------------------------------------------------------------------------------------------------------------------------------+--------------------+
| note_text                                                                                                                                          | ranks              |
+----------------------------------------------------------------------------------------------------------------------------------------------------+--------------------+
| Safe is combination locked, combination not provided with safe.This is rarely a problem as safes are typically blown up or dropped by customers.   | 0.8333333730697632 |
+----------------------------------------------------------------------------------------------------------------------------------------------------+--------------------+
1 row in set (0.00 sec)
```

### 5.全文本搜索的使用说明

1. 在索引全文本数据时，短词被忽略且从索引中排除。短词定义为那些具有 3 个或 3 个以下字符的词
2. MySQL 带有一个内建的非用词（stopword）列表，这些词在索引全文本数据时总是被忽略。[MySql 文档](https://dev.mysql.com/doc/refman/5.7/en/fulltext-stopwords.html)
3. 许多词出现的频率很高，搜索它们没有用处（返回太多的结果）。因此，MySQL 规定了一条 50%规则，如果一个词出现在 50%以上的行中，则将它作为一个非用词忽略。50%规则不用于 IN BOOLEAN MODE。
4. 如果表中的行数少于 3 行，则全文本搜索不返回结果
5. 忽略词中的单引号。例如，don't 索引为 dont。
6. 不具有词分隔符（包括日语和汉语）的语言不能恰当地返回全文本搜索结果。
7. 仅在 MyISAM 数据库引擎中支持全文本搜索

## 第 19 章：插入数据

**插入完整的行**

```sql
-- 要按照表结构的顺序，不安全，不建议
INSERT INTO Customers VALUES (NULL, 'Pep E. LaPew', '100 Main Street', 'Los Angeles','CA', '90046','USA',NULL,NULL);

-- 自己定义字段的顺序和数量，比较安全，建议
INSERT INTO customers(cust_name,cust_address, cust_city, cust_state, cust_zip, cust_country, cust_contact,cust_email )
VALUES('Pep E. LaPew','100 Main Street','Los Angeles ','CA','90046','USA', NULL, NULL) ;
```

**插入多个行**

```sql
-- 方式一
INSERT INTO customers (cust_name,cust_address,cust_city,cust_state,cust_zip,cust_country)
VALUES('Pep E. LaPew','100 Main Street', 'Los Angeles', 'CA', '90046', 'USA') ;
INSERT INTO customers (cust_name,cust_address,cust_city,cust_state,cust_zip,cust_country)
VALUES('M. Martian','42 Galaxy Way','New York','NY','11213','USA');

-- 方式二：这个性能更好，MySQL用单条INSERT语句处理多个插入比使用多条INSERT语句快。
INSERT INTO customers (cust_name,cust_address,cust_city,cust_state,cust_zip,cust_country)
VALUES('Pep E. LaPew','100 Main Street', 'Los Angeles', 'CA', '90046', 'USA'),
('M. Martian','42 Galaxy Way','New York','NY','11213','USA');

```

**插入检索出的数据 INSERT SELECT**

```sql
INSERT INTO custnew (cust_id,cust_contact,cust_email,cust_name,cust_address,cust_city,cust_state,cust_zip,cust_country)
SELECT cust_id,cust_contact,cust_email,cust_name,cust_address,cust_city,cust_state,cust_zip,cust_country FROM customers;
```

提高整体性能：如果数据检索是最重要的（通常是这样），则你可以通过在 INSERT 和 INTO 之间添加关键字 LOW_PRIORITY，指示 MySQL 降低 INSERT 语句的优先级，
如下所示：INSERT LOW_PRIORITY INTO。这也适用于 UPDATE 和 DELETE 语句。

INSERT SELECT 是根据位置来填充值的，而不是列名

## 第 20 章：更新和删除数据

```sql
-- 更新一列
UPDATE customers SET cust_email = 'elmer@fudd.com' WHERE cust_id = 10005;
-- 更新多列
UPDATE customers SET cust_name = 'The Fudds', cust_email = 'elmer@fudd.com' WHERE cust_id = 10005;
-- NULL用来去除某列中的值
UPDATE customers SET cust_email = NULL WHERE cust_id = 10005;
```

```sql
-- DELETE是一行一行删除
DELETE FROM customers WHERE cust_id = 10006;
-- truncate 删除原来的表并重新创建一个表, 效率高
truncate table customers

```

## 第 21 章：创建和操纵表

1. 创建表

   ```sql
   CREATE TABLE customers
   (
   cust_id      int       NOT NULL AUTO_INCREMENT,
   cust_name    char(50)  NOT NULL ,
   cust_address char(50)  NULL ,
   cust_city    char(50)  NULL ,
   cust_state   char(5)   NULL ,
   cust_zip     char(10)  NULL ,
   cust_country char(50)  NULL ,
   cust_contact char(50)  NULL ,
   cust_email   char(255) NULL ,
   PRIMARY KEY (cust_id)
   ) ENGINE=InnoDB;

   CREATE TABLE orderitems
   (
   order_num  int          NOT NULL ,
   order_item int          NOT NULL ,
   prod_id    char(10)     NOT NULL ,
   quantity   int          NOT NULL  DEFAULT 0,
   item_price decimal(8,2) NOT NULL ,
   PRIMARY KEY (order_num, order_item)
   ) ENGINE=InnoDB;
   ```

   - CREATE TABLE 表名：创建表()
     - 如果重复创建`CREATE TABLE customers2 (...)`，数据库会报错：`1050 - Table 'customers2' already exists`
     - `CREATE TABLE  IF NOT EXISTS customers2` 加上`IF NOT EXISTS`之后，就不会报错，因为如果不存在才去创建
   - NOT NULL 不允许为 NULL，NULL 允许为 NULL（默认）
   - PRIMARY KEY 指定主键，一个主键或者多个主键
     - PRIMARY KEY (order_num, order_item) 意味着 order_num 与 order_item 作为一个整体来判断是否一致
   - AUTO_INCREMENT 自增
     - 如果一个 id 为 1000 的值修改为 2000,之后再插入数据的 id 就是 2001
     - 插入后如何获取自增 id：`SELECT LAST_INSERT_ID()`
       - [参考文章 1](https://blog.csdn.net/Soinice/article/details/88845898)
       - [参考文章 2](https://www.yiibai.com/mysql/last_insert_id.html)
       - [参考文章 3](https://juejin.cn/post/7013358349735428126)
   - DEFAULT 指定默认值
   - ENGINE=InnoDB 指定引擎
     - InnoDB 是一个可靠的事务处理引擎，它不支持全文本搜索（5.5 之后默认）
     - MEMORY 在功能等同于 MyISAM，但由于数据存储在内存（不是磁盘）中，速度很快（特别适合于临时表）
     - MyISAM 是一个性能极高的引擎，它支持全文本搜索，但不支持事务处理（5.5 之前默认）

2. 更新表

   ```sql
   --  新增列
   ALTER TABLE vendors ADD vend_phone CHAR(20) ;

   -- 删除列
   ALTER TABLE vendors DROP COLUMN vend_phone;

   -- 增加外键
   ALTER TABLE orderitems ADD CONSTRAINT fk_orderitems_orders FOREIGN KEY (order_num) REFERENCES orders (order_num);

   ```

3. 重命名表

   ```sql
   RENAME TABLE customers2 TO customers3;
   ```

4. 删除表
   ```sql
   DROP TABLE customers3;
   ```

## 第 22 章：使用视图

1. 什么是视图

   - 视图仅仅是用来查看存储在别处的数据的一种设施
   - 视图是一种虚拟表，视图本身不包含数据
   - 对视图的新增、更新、删除会影响其基表的数据，对基表数据的新增、更新、删除同样会影响视图
   - 因为视图不包含数据，所以每次使用视图时，其实还是执行了一次构成视图的查询语句

2. 视图的创建

   - 利用视图简化复杂的联结

   ```sql
   CREATE VIEW productcustomers AS
   SELECT cust_name, cust_contact, prod_id
   FROM customers, orders, orderitems
   WHERE customers.cust_id = orders.cust_id
   AND orderitems.order_num = orders.order_num;

   mysql> SELECT * FROM productcustomers;
   +----------------+--------------+---------+
   | cust_name      | cust_contact | prod_id |
   +----------------+--------------+---------+
   | Coyote Inc.    | Y Lee        | ANV01   |
   | Coyote Inc.    | Y Lee        | ANV02   |
   | Coyote Inc.    | Y Lee        | TNT2    |
   | Coyote Inc.    | Y Lee        | FB      |
   | Coyote Inc.    | Y Lee        | FB      |
   | Coyote Inc.    | Y Lee        | OL1     |
   | Coyote Inc.    | Y Lee        | SLING   |
   | Coyote Inc.    | Y Lee        | ANV03   |
   | Wascals        | Jim Jones    | JP2000  |
   | Yosemite Place | Y Sam        | TNT2    |
   | The Fudds      | E Fudd       | FC      |
   +----------------+--------------+---------+
   11 rows in set (0.00 sec)

   ```

   - 用视图重新格式化检索出的数据

   ```sql
   mysql> CREATE VIEW vendorlocations AS
      -> SELECT Concat(RTrim(vend_name),' (', RTrim(vend_country),')')
      -> AS vend_title
      -> FROM vendors
      -> ORDER BY vend_name;
   Query OK, 0 rows affected (0.01 sec)

   mysql> SELECT * FROM vendorlocations;
   +-------------------------+
   | vend_title              |
   +-------------------------+
   | ACME (USA)              |
   | Anvils R Us (USA)       |
   | Furball Inc. (USA)      |
   | Jet Set (England)       |
   | Jouets Et Ours (France) |
   | LT Supplies (USA)       |
   +-------------------------+
   6 rows in set (0.00 sec)
   ```

   - 用视图过滤不想要的数据

   ```sql
   mysql> CREATE VIEW customeremaillist AS
      -> SELECT cust_id,cust_name, cust_email
      -> FROM customers
      -> WHERE cust_email IS NOT NULL ;
   Query OK, 0 rows affected (0.01 sec)

   mysql> SELECT * FROM customeremaillist;
   +---------+----------------+---------------------+
   | cust_id | cust_name      | cust_email          |
   +---------+----------------+---------------------+
   |   10001 | Coyote Inc.    | ylee@coyote.com     |
   |   10003 | Wascals        | rabbit@wascally.com |
   |   10004 | Yosemite Place | sam@yosemite.com    |
   |   10005 | The Fudds      | elmer@fudd.com      |
   +---------+----------------+---------------------+
   4 rows in set (0.00 sec)
   ```

   - 使用视图与计算字段

   ```sql
   mysql> CREATE VIEW orderitemsexpanded AS
      -> SELECT order_num,prod_id,quantity,item_price,quantity*item_price AS expanded_price
      -> FROM orderitems ;
   Query OK, 0 rows affected (0.01 sec)

   mysql> SELECT * FROM orderitemsexpanded;
   +-----------+---------+----------+------------+----------------+
   | order_num | prod_id | quantity | item_price | expanded_price |
   +-----------+---------+----------+------------+----------------+
   |     20005 | ANV01   |       10 |       5.99 |          59.90 |
   |     20005 | ANV02   |        3 |       9.99 |          29.97 |
   |     20005 | TNT2    |        5 |      10.00 |          50.00 |
   |     20005 | FB      |        1 |      10.00 |          10.00 |
   |     20006 | JP2000  |        1 |      55.00 |          55.00 |
   |     20007 | TNT2    |      100 |      10.00 |        1000.00 |
   |     20008 | FC      |       50 |       2.50 |         125.00 |
   |     20009 | FB      |        1 |      10.00 |          10.00 |
   |     20009 | OL1     |        1 |       8.99 |           8.99 |
   |     20009 | SLING   |        1 |       4.49 |           4.49 |
   |     20009 | ANV03   |        1 |      14.99 |          14.99 |
   +-----------+---------+----------+------------+----------------+
   11 rows in set (0.00 sec)
   ```

3. `SHOW CREATE VIEW customeremaillist;`来查看创建视图的语句

4. `DROP VIEW productcustomers;` 删除视图

5. 更新视图

   ```sql
   mysql> CREATE OR REPLACE VIEW vendorlocations AS
      -> SELECT Concat(RTrim(vend_name),' (', RTrim(vend_country),')----')
      -> AS vend_title
      -> FROM vendors
      -> ORDER BY vend_name;
   Query OK, 0 rows affected (0.01 sec)

   mysql> SELECT * FROM vendorlocations;
   +-----------------------------+
   | vend_title                  |
   +-----------------------------+
   | ACME (USA)----              |
   | Anvils R Us (USA)----       |
   | Furball Inc. (USA)----      |
   | Jet Set (England)----       |
   | Jouets Et Ours (France)---- |
   | LT Supplies (USA)----       |
   +-----------------------------+
   6 rows in set (0.00 sec)
   ```

   如果要更新的视图不存在, 就会新建视图

   如果要更新的视图存在，就会更新视图

6. 对视图进行增删改查

   在 MySQL5 中 如果视图定义中有以下操作，则不能进行视图的增删改查：

   - 分组（使用 GROUP BY 和 HAVING）；
   - 联结 （在 MySQL8 中试了，可以）
   - 子查询
   - 并
   - 聚集函数（Min()、Count()、Sum()等
   - DISTINCT
   - 导出（计算）列

   **一般，应该将视图用于检索（SELECT 语句）而不用于更新（INSERT、UPDATE 和 DELETE）。**

## 第 23 章：使用存储过程

### 1.如何理解存储过程

1. 存储过程是什么：存储过程简单来说，就是为以后的使用而保存的一条或多条 MySQL 语句的集合。

2. 为什么要使用存储过程：好处是把复杂的 SQL 代码都封装起来，这样带来以下 3 个好处
   - 简化复杂的操作
   - 保证了数据的完整性
   - 简化对变动的管理

### 2.创建存储过程

```sql
-- 如果再图形界面操作，可以这么写
CREATE PROCEDURE productpricing()
BEGIN
SELECT Avg(prod_price) AS priceaverage
FROM products;
END ;

-- 如果再MySQL的命令行操作，需要重新定义分隔符
-- DELIMITER // 把分隔符定义为 //
-- END // 再end后面执行完
-- DELIMITER ; 紧接着再把分隔符改回 ;
mysql> DELIMITER //
mysql> CREATE PROCEDURE productpricing()
    -> BEGIN
    -> SELECT Avg(prod_price) AS priceaverage
    -> FROM products;
    -> END //
Query OK, 0 rows affected (0.02 sec)

mysql> DELIMITER ;
```

### 3.执行存储过程

```sql
mysql> CALL productpricing();
+--------------+
| priceaverage |
+--------------+
|    16.133571 |
+--------------+
1 row in set (0.03 sec)

Query OK, 0 rows affected (0.04 sec)
```

### 4.删除存储过程

```sql
-- 删除
mysql> DROP PROCEDURE productpricing;
Query OK, 0 rows affected (0.01 sec)

-- 仅当存在时删除
mysql> DROP PROCEDURE productpricing;
ERROR 1305 (42000): PROCEDURE temp.productpricing does not exist
mysql> DROP PROCEDURE IF EXISTS productpricing;
Query OK, 0 rows affected, 1 warning (0.00 sec)
```

### 4.使用参数

```sql
-- 创建存储过程
CREATE PROCEDURE productpricing(
OUT pl DECIMAL(8,2),
OUT ph DECIMAL(8,2),
OUT pa DECIMAL(8,2)
)
BEGIN
	SELECT Min(prod_price)
	INTO pl
	FROM products;
	SELECT
	Max(prod_price)
	INTO ph
	FROM
	products;
	SELECT Avg(prod_price)
	INTO pa
	FROM products;
END;
-- 调用存储过程，并将结果存储在 @pricelow,@pricehigh,@priceaverage 三个变量中
mysql> CALL productpricing(@pricelow,@pricehigh,@priceaverage);
Query OK, 1 row affected, 1 warning (0.00 sec)
-- 访问变量，获得结果
mysql> SELECT @pricelow,@pricehigh,@priceaverage;
+-----------+------------+---------------+
| @pricelow | @pricehigh | @priceaverage |
+-----------+------------+---------------+
|      2.50 |      55.00 |         16.13 |
+-----------+------------+---------------+
1 row in set (0.00 sec)

```

- 变量/参数 是按照顺序一一对应的，所以顺序要注意，数量不能变
- IN（传递给存储过程）
- OUT（从存储过程传出，如这里所用）
- INOUT（对存储过程传入和传出）
- INTO 将查询出来的值存储到变量中

**【发现如下奇怪现象】**

```sql
-- 1. 我先是在Navicate里面创建了存储过程
-- 2. 接着又调用存储过程
-- 3. 然后再访问变量（SELECT），可以正常得到值
-- 4. 接着我用MySQL自带的命令行访问变量（SELECT），得到的却是NULL
mysql> SELECT @pricelow,@pricehigh,@priceaverage;
+----------------------+------------------------+------------------------------+
| @pricelow            | @pricehigh             | @priceaverage                |
+----------------------+------------------------+------------------------------+
| NULL                 | NULL                   | NULL                         |
+----------------------+------------------------+------------------------------+
1 row in set (0.00 sec)

-- 于是猜想一：存储过程是放在客户端上的？结果执行如下命令
mysql> SHOW CREATE PROCEDURE productpricing ;
-- 发现两边的存储过程的内容是一样的

-- 接着猜想二：存储的变量是放在客户端的？
-- 于是我在MySQL的自带的客户端再次执行调用命令：
CALL productpricing(@pricelow,@pricehigh,@priceaverage);
-- 接着再查询，就又得到了结果

-- 很可能存储的变量是放在客户端上面，Navicate 和Mysql自带的客户端不是同一个客户端

```

```sql
CREATE PROCEDURE ordertotal(
	IN onumber INT ,
	OUT otota1 DECIMAL(8,2)
)
BEGIN
	SELECT Sum(item_price*quantity)
	FROM orderitems
	WHERE order_num = onumber
	INTO otota1 ;
END;

mysql> CALL ordertotal(20005,@total);
Query OK, 1 row affected (0.01 sec)

mysql> SELECT @total;
+--------+
| @total |
+--------+
| 149.87 |
+--------+
1 row in set (0.00 sec)

```

### 5.建立智能存储过程(包含业务的，更复杂的存储过程)

【业务描述】你需要获得与以前一样的订单合计，但需要对合计增加营业税，不过只针对某些顾客（或许是你所在州中那些顾客）。那么，你需要做下面几件事情：

- 获得合计（与以前一样）；
- 把营业税有条件地添加到合计；
- 返回合计（带或不带税）。

```sql
-- Name: ordertotal
-- Parameters:  onumber = order number
-- 							taxable = 0 if not taxable, l if taxable
-- 							ototal = order total variable
CREATE PROCEDURE ordertotal(
IN onumber INT,
IN taxable BOOLEAN,
OUT ototal DECIMAL(8,2)
) COMMENT 'Obtain order total,optionally adding tax'

BEGIN
   -- Declare variable for total
   DECLARE total DECIMAL(8,2);
   -- Declare tax percentage
   DECLARE taxrate INT DEFAULT 6;
   -- Get the order total
   SELECT Sum(item_price*quantity)
   FROM orderitems
   WHERE order_num = onumber
   INTO total;

   IF taxable THEN
      -- Is this taxable?-- Yes，SO add taxrate to the tota l
      SELECT total+(total /100*taxrate) INTO total ;
   END IF;
      -- And finally， save to out variable
      SELECT total INTO ototal ;

END;

mysql> CALL ordertotal (20005, 0, @total);
Query OK, 1 row affected (0.00 sec)

mysql> SELECT @total ;
+--------+
| @tota1 |
+--------+
| 149.87 |
+--------+
1 row in set (0.00 sec)
```

1. SHOW PROCEDURE STATUS 可以查到 Comment

2. BOOLEAN 值指定为 1 表示真（非 0 都为真），指定为 0 表示假

3. IF A THEN SQL 语句 END IF : 如果 A 为真，就执行 SQL 语句

4. 用 DECLARE 语句定义了两个局部变量。DECLARE 要求指定变量名和数据类型，它也支持可选的默认值

### 6.检查存储过程

```sql
SHOW CREATE PROCEDURE ordertotal ;
SHOW PROCEDURE STATUS WHERE Name= 'ordertotal'
```

## 第 24 章：使用游标

游标（cursor）是一个存储在 MySQL 服务器上的数据库查询，它不是一条 SELECT 语句，而是被该语句检索出来的结果集。在存储了游标之后，应用程序可以根据需要滚动或浏览其中的数据。

相当于 SELECT 结果的集合

```sql
CREATE PROCEDURE processorders()
BEGIN
   -- Declare local variables
   DECLARE done BOOLEAN DEFAULT 0;
   DECLARE o INT;
   DECLARE t DECIMAL (8,2);


   -- Declare the cursor
   DECLARE ordernumbers CURSOR
   FOR
   SELECT order_num FROM orders;

   /*3.*/
   -- Declare continue handler
   DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET done=1;
   -- Create a table to store the results
   CREATE TABLE IF NOT EXISTS ordertotals
   (order_num INT, total DECIMAL(8,2));

   -- Open the cursor
   OPEN ordernumbers;

		 -- Loop through all rows
		 REPEAT
			 -- Get order number
			 FETCH ordernumbers INTO o;

			 -- Get the total for this order
			 CALL ordertotal(o, 1, t);
			 --
			 -- Insert order and total into ordertotals
			 INSERT INTO ordertotals (order_num, total)
			 VALUES(o, t);

		 -- End of loop
		 UNTIL done END REPEAT;
   -- Close the cursor
   CLOSE ordernumbers;
END;

mysql> CALL processorders();
Query OK, 1 row affected (0.04 sec)

mysql> SELECT * FROM ordertotals;
+-----------+---------+
| order_num | total   |
+-----------+---------+
|     20005 |  158.86 |
|     20009 |   40.78 |
|     20006 |   58.30 |
|     20007 | 1060.00 |
|     20008 |  132.50 |
|     20008 |  132.50 |
+-----------+---------+
6 rows in set (0.00 sec)

```

1. MySQL 游标只能用于存储过程（和函数）
2. `DECLARE ordernumbers CURSOR`: 声明游标，这个游标装着（FOR） SELECT order_num FROM orders 的结果集
3. `DECLARE CONTINUE HANDLER`定义了一个 CONTINUE HANDLER, 当满足`FOR SQLSTATE '02000'`这个条件时，会执行 `SET done=1`
   - SQLSTATE 是 sql 执行的错误码，可见[官方文档](https://dev.mysql.com/doc/connector-j/8.0/en/connector-j-reference-error-sqlstates.html), 02000 表示未找到符合条件的数据
   - `done`在`UNTIL done END REPEAT;`中用到了，0 表示假，非 0 表示真。
   - done 是代码开头定义的`DECLARE done BOOLEAN DEFAULT 0;`
4. `OPEN ordernumbers`开启路由
5. `REPEAT`重复执行代码，除了`REPEAT`还有`LEAVE`。作者推荐用`REPEAT`
6. `UNTIL done END REPEAT;` 直到 done 为真时，结束循环
7. `FETCH ordernumbers INTO o;` FETCH 用来检索当前行的 order_num 列（将自动从第一行开始）到一个名为 o 的局部声明的变量中
8. `CALL ordertotal(o, 1, t);` 执行上一章生成的存储过程
9. `CLOSE ordernumbers;` 关闭游标。如果你不明确关闭游标，MySQL 将会在到达`END`语句时自动关闭它。

## 第 25 章：使用触发器

1. 触发器是 MySQL 响应 DELETE、INSERT 或者 UPDATE 语句而自动执行的一条或多条 MySQL 语句

2. 创建触发器

   ```sql
   -- MySQL5早期版本是支持的，现在的新版本已经不支持这种写法。
   -- 触发器不允许出现SELECT *的形式，因为这会返回一个结果集，而这是不允许的，所以会报出这种错。
   mysql> CREATE TRIGGER newproduct AFTER INSERT ON products
      -> FOR EACH ROW SELECT 'Product added' ;
   ERROR 1415 (0A000): Not allowed to return a result set from a trigger

   -- 发器中可以使用SELECT  INTO的形式来进行查询，将结果放进一个变量，然后查询该变量。
   mysql> CREATE TRIGGER newproduct AFTER INSERT ON products
      -> FOR EACH ROW SELECT 'Product added' INTO @arg;
   Query OK, 0 rows affected (0.02 sec)

   mysql> INSERT INTO products VALUES('TNT3',1003,'TNT (6 sticks)',10,'test');
   Query OK, 1 row affected (0.01 sec)

   mysql> SELECT @arg;
   +---------------+
   | @arg          |
   +---------------+
   | Product added |
   +---------------+
   1 row in set (0.00 sec)

   ```

   - 在 MySQL 5 中，触发器名必须在每个表中唯一，但不是在每个数据库中唯一。但是作者建议保持每个数据库的触发器名唯一
   - 只有表才支持触发器，视图不支持
   - 每个表最多支持 6 个触发器（每条 INSERT、UPDATE 和 DELETE 的之前和之后）
   - 如果 BEFORE 触发器失败，则 MySQL 将不执行请求的操作，也不执行 AFTER 触发器
   - CREATE TRIGGER 名字： 创建触发器
   - AFTER INSERT 在 INSERT 之后执行
   - FOR EACH ROW 代码对每个插入行执行（也可以是删除或者修改）

3. 删除触发器`DROP TRIGGER newproduct;`

4. INSERT 触发器

   ```sql
   mysql> CREATE TRIGGER neworder AFTER INSERT ON orders
      -> FOR EACH ROW SELECT NEW.order_num;
   ERROR 1415 (0A000): Not allowed to return a result set from a trigger
   ```

   ```sql
   mysql> CREATE TRIGGER neworder AFTER INSERT ON orders
      -> FOR EACH ROW SELECT NEW.order_num INTO @arg;
   Query OK, 0 rows affected (0.01 sec)

   mysql> INSERT INTO orders(order_date,cust_id)
      -> VALUES (Now(),10001) ;
   Query OK, 1 row affected (0.01 sec)

   mysql> SELECT @arg;
   +-------+
   | @arg  |
   +-------+
   | 20010 |
   +-------+
   1 row in set (0.00 sec)
   ```

   - 在 INSERT 触发器代码内，可引用一个名为 NEW 的虚拟表，访问被插入的行；
   - 在 BEFORE INSERT 触发器中，NEW 中的值也可以被更新（允许更改被插入的值）；
   - 对于 AUTO_INCREMENT 列，NEW 在 INSERT 执行之前包含 0，在 INSERT 执行之后包含新的自动生成值。

5. DELETE 触发器

   ```sql
   CREATE TRIGGER deleteorder BEFORE DELETE ON orders
   FOR EACH ROW
   BEGIN
      INSERT INTO archive_orders (order_num, order_date, cust_id)
      VALUES(OLD.order_num，OLD.order_date, OLD.cust_id) ;
   END;
   ```

   - 这个有个`;`，所以不能直接在命令行里面执行
   - 在 DELETE 触发器代码内，你可以引用一个名为 OLD 的虚拟表，访问被删除的行；
   - OLD 中的值全都是只读的，不能更新。
   - BEGIN ... END 可以容纳多条 SQL 语句，虽然这里只写了一条

6. UPDATE 触发器

   ```sql
   mysql> CREATE TRIGGER updatevendor BEFORE UPDATE ON vendors
      -> FOR EACH ROW SET NEW.vend_state = Upper(NEW.vend_state);
   Query OK, 0 rows affected (0.03 sec)

   mysql> UPDATE vendors SET vend_state = 'aaa' WHERE vend_id = 1005;
   Query OK, 1 row affected (0.00 sec)
   Rows matched: 1  Changed: 1  Warnings: 0

   mysql> SELECT vend_id, vend_state FROM vendors WHERE vend_id = 1005;
   +---------+------------+
   | vend_id | vend_state |
   +---------+------------+
   |    1005 | AAA        |
   +---------+------------+
   1 row in set (0.00 sec)
   ```

   - 在 UPDATE 触发器代码中，你可以引用一个名为 OLD 的虚拟表访问以前（UPDATE 语句前）的值，引用一个名为 NEW 的虚拟表访问新更新的值；
   - 在 BEFORE UPDATE 触发器中，NEW 中的值可能也被更新（允许更改将要用于 UPDATE 语句中的值）；
   - OLD 中的值全都是只读的，不能更新。

## 第 26 章：管理事务处理

1. 事务处理: 要么全部成功，要么全部失败
2. 事务（transaction）指一组 SQL 语句；
3. 回退（rollback）指撤销指定 SQL 语句的过程
4. 提交（commit）指将未存储的 SQL 语句结果写入数据库表；
5. 保留点（savepoint）指事务处理中设置的临时占位符（placeholder），你可以对它发布回退,也就是回退到保留点的位置，而不是全部回退。
6. 使用 ROLLBACK

   ```sql
   mysql> SELECT * FROM ordertotals;
   +-----------+---------+
   | order_num | total   |
   +-----------+---------+
   |     20005 |  158.86 |
   |     20009 |   40.78 |
   |     20006 |   58.30 |
   |     20007 | 1060.00 |
   |     20008 |  132.50 |
   |     20008 |  132.50 |
   +-----------+---------+
   6 rows in set (0.02 sec)

   mysql> START TRANSACTION;
   Query OK, 0 rows affected (0.00 sec)

   mysql> DELETE FROM ordertotals;
   Query OK, 6 rows affected (0.00 sec)

   mysql> SELECT * FROM ordertotals;
   Empty set (0.00 sec)

   mysql> ROLLBACK;
   Query OK, 0 rows affected (0.00 sec)

   mysql> SELECT * FROM ordertotals;
   +-----------+---------+
   | order_num | total   |
   +-----------+---------+
   |     20005 |  158.86 |
   |     20009 |   40.78 |
   |     20006 |   58.30 |
   |     20007 | 1060.00 |
   |     20008 |  132.50 |
   |     20008 |  132.50 |
   +-----------+---------+
   6 rows in set (0.00 sec)
   ```

   - START TRANSACTION 开启事物
   - ROLLBACK 回退 START TRANSACTION 之后的命令

7. 使用 COMMIT

   ```sql
   mysql> START TRANSACTION;
   Query OK, 0 rows affected (0.00 sec)

   mysql> SELECT * FROM orderitems  WHERE order_num = 20008;
   +-----------+------------+---------+----------+------------+
   | order_num | order_item | prod_id | quantity | item_price |
   +-----------+------------+---------+----------+------------+
   |     20008 |          1 | FC      |       50 |       2.50 |
   +-----------+------------+---------+----------+------------+
   1 row in set (0.00 sec)

   mysql> DELETE FROM orderitems WHERE order_num = 20008;
   Query OK, 1 row affected (0.02 sec)

   mysql> SELECT * FROM orderitems  WHERE order_num = 20008;
   Empty set (0.00 sec)

   mysql>
   mysql> SELECT * FROM orders WHERE order_num = 20008;
   +-----------+---------------------+---------+
   | order_num | order_date          | cust_id |
   +-----------+---------------------+---------+
   |     20008 | 2005-10-03 00:00:00 |   10005 |
   +-----------+---------------------+---------+
   1 row in set (0.00 sec)

   mysql> DELETE FROM orders WHERE order_num = 20008;
   Query OK, 1 row affected (0.00 sec)

   mysql> SELECT * FROM orders WHERE order_num = 20008;
   Empty set (0.00 sec)

   mysql> COMMIT ;
   Query OK, 0 rows affected (0.02 sec)
   ```

   - 隐含提交（implicit commit），即提交（写或保存）操作是自动进行的。
   - 在事务处理块中，提交不会隐含地进行。为进行明确的提交，使用 COMMIT 语句
   - 当 COMMIT 或 ROLLBACK 语句执行后，事务会自动关闭（将来的更改会隐含提交）。

8. 使用保留点

   ```sql
   mysql> START TRANSACTION;
   Query OK, 0 rows affected (0.00 sec)

   mysql> UPDATE orderitems SET quantity = 11 WHERE prod_id='ANV01';
   Query OK, 1 row affected (0.00 sec)
   Rows matched: 1  Changed: 1  Warnings: 0

   mysql> SAVEPOINT update1;
   Query OK, 0 rows affected (0.00 sec)

   mysql> UPDATE ordertotals SET total=100 WHERE order_num = 20005 ;
   Query OK, 1 row affected (0.00 sec)
   Rows matched: 1  Changed: 1  Warnings: 0

   mysql> ROLLBACK TO update1;
   Query OK, 0 rows affected (0.00 sec)

   mysql> SELECT * FROM  orderitems WHERE prod_id='ANV01';
   +-----------+------------+---------+----------+------------+
   | order_num | order_item | prod_id | quantity | item_price |
   +-----------+------------+---------+----------+------------+
   |     20005 |          1 | ANV01   |       11 |       5.99 |
   +-----------+------------+---------+----------+------------+
   1 row in set (0.00 sec)

   mysql> SELECT * FROM  ordertotals WHERE  order_num = 20005;
   +-----------+--------+
   | order_num | total  |
   +-----------+--------+
   |     20005 | 158.86 |
   +-----------+--------+
   1 row in set (0.00 sec)
   ```

   - 可以回退到保留点位置
   - 保留点越多越好
   - 保留点在事务处理完成（执行一条 ROLLBACK 或 COMMIT）后自动释放。
   - 也可以用`RELEASE SAVEPOINT`明确地释放保留点。

9. 更改默认的提交行为: `SET autocommit = 0;`。设置 autocommit 为 0（假）指示 MySQL 不自动提交更改

## 第 27 章：全球化和本地化（字符集问题）

1. 字符集为字母和符号的集合；
2. 编码为某个字符集成员的内部表示；
3. 校对为规定**字符如何比较**的指令。

   - 影响排序（如用 ORDER BY 排序数据）
   - 影响搜索（例如，寻找 apple 的 WHERE 子句是否能找到 APPLE）

4. 显示所有可用的字符集以及每个字符集的描述和默认校对。

```sql
SHOW CHARACTER SET;

SHOW CHARACTER SET LIKE 'UTF%';
```

5. 显示所有可用的校对，以及它们适用的字符集。可以看到有的字符集具有不止一种校对

```sql
SHOW COLLATION;
SHOW COLLATION LIKE 'UTF%';
```

6. 通常系统管理在安装时定义一个默认的字符集和校对
7. 也可以在创建数据库时，指定默认的字符集和校对
8. 不同的表，甚至不同的列都可能需要不同的字符集

```sql
CREATE TABLE mytable
(
column1 INT,
column2 VARCHAR(10),
column3 VARCHAR(10) CHARACTER SET latin1 COLLATE latin1_general_ci
) DEFAULT CHARACTER SET hebrew
COLLATE hebrew_general_ci ;
```

- 如果指定 CHARACTER SET 和 COLLATE 两者，则使用这些值。
- 如果只指定 CHARACTER SET，则使用此字符集及其默认的校对（如 SHOW CHARACTER SET 的结果中所示）。
- 如果既不指定 CHARACTER SET，也不指定 COLLATE，则使用数据库默认

9. 校对在对用 ORDER BY 子句检索出来的数据排序时起重要的作用。如果你需要用与创建表时不同的校对顺序排序特定的 SELECT 语句，可以在 SELECT 语句自身中进行：**【已经不可以了】**

```sql
-- 已经不可以这么设置了
SELECT * FROM customers ORDER BY cust_name COLLATE  latin1_general_cs;
> 1253 - COLLATION 'latin1_general_cs' is not valid for CHARACTER SET 'utf8mb4'

-- 发现建表的时候设置的是  COLLATE=utf8mb4_0900_ai_ci
show create table customers;

-- 之后也只能这么查询
SELECT * FROM customers ORDER BY cust_name COLLATE utf8mb4_0900_ai_ci;
```

## 第 28 章：安全管理（管理用户和授权）

1. 查看 MySQL 的用户

```sql
-- MySQL用户账号和信息存储在名为mysql的MySQL数据库中
use mysql;
SELECT user FROM user
```

2. 创建用户账号 `CREATE USER ben IDENTIFIED BY 'ben';`

3. 给账户重命名`RENAME USER ben TO ben2;`

4. 删除用户`DROP USER ben2;`

5. 设置访问权限

- 展示权限

```sql
-- USAGE表示根本没有权限
mysql> SHOW GRANTS FOR ben;
+---------------------------------+
| Grants for ben@%                |
+---------------------------------+
| GRANT USAGE ON *.* TO `ben`@`%` |
+---------------------------------+
1 row in set (0.00 sec)
```

MySQL 的权限用用户名和主机名结合定义。如果不指定主机名，则使用默认的主机名%（授予用户访问权限而不管主机名）。

- 授予权限`GRANT SELECT ON temp.* TO ben;`
- 撤销权限`REVOKE SELECT ON temp.* FROM ben;`
  - GRANT 和 REVOKE 可在几个层次上控制访问权限：
  - 整个服务器，使用 GRANT ALL 和 REVOKE ALL；
  - 整个数据库，使用 ON database.\*；
  - 特定的表，使用 ON database.table；
  - 特定的列；
  - 特定的存储过程
- 简化多次授权`GRANT SELECT,UPDATE ON temp.* TO ben;`

6. 更改口令

   ```sql
   -- mysql 5.7.9 之后取消了password 函数，所以执行这个SQL会报错
   SET PASSWORD FOR ben = password('ben123');
   -- 正确的方式
   ALTER USER 'ben' IDENTIFIED WITH mysql_native_password BY 'ben123';

   ```

[MySQL8.0 正确修改密码的姿势--通俗易懂](https://cloud.tencent.com/developer/article/2115190)

[MySQL 8.0 修改密码](https://www.cnblogs.com/chloneda/p/12449819.html)

## 第 29 章：数据库维护

1. 备份数据

2. 进行数据库维护

   ```sql
   -- ANALYZE TABLE，检查表是否正常
   mysql> ANALYZE TABLE orders;
   +-------------+---------+----------+----------+
   | Table       | Op      | Msg_type | Msg_text |
   +-------------+---------+----------+----------+
   | temp.orders | analyze | status   | OK       |
   +-------------+---------+----------+----------+
   1 row in set (0.03 sec)
   -- CHECK TABLE，检查表是否正常
   mysql> CHECK TABLE orders,orderitems;
   +-----------------+-------+----------+----------+
   | Table           | Op    | Msg_type | Msg_text |
   +-----------------+-------+----------+----------+
   | temp.orders     | check | status   | OK       |
   | temp.orderitems | check | status   | OK       |
   +-----------------+-------+----------+----------+
   2 rows in set (0.01 sec)
   ```

3. 诊断启动问题

   - mysqld --help 显示帮助——一个选项列表；
   - mysqld --safe-mode 装载减去某些最佳配置的服务器；
   - mysqld --verbose 显示全文本消息（为获得更详细的帮助消息与--help 联合使用）；
   - mysqld --version 显示版本信息然后退出。

4. 查看日志文件

- 错误日志：日志通常名为 hostname.err，位于 data 目录中。此日志名可用 --log-error 命令行选项更改。
- 查询日志：此日志通常名为 hostname.log，位于 data 目录中。此名字可以用--log 命令行选项更改。
- 二进制日志：此日志通常名为 hostname-bin，位于 data 目录内。此名字可以用--log-bin 命令行选项更改
- 缓慢查询日志：此日志**记录执行缓慢的任何查询**。这个日志在确定数据库何处需要优化很有用。此日志通常名为 hostname-slow.log ， 位于 data 目录中。此名字可以用--log-slow-queries 命令行选项更改。

## 第 30 章：改善性能

1. MySQL 具有特定的硬件要求
2. 关键的生产 DBMS 应该运行在自己的专用服务器上。
3. 可以通过修改一些配置来提升性能（内存分配、缓冲区大小等）
   - `SHOW VARIABLES;`和`SHOW STATUS;`可以查看当前设置
4. MySQL 一个多用户多线程的 DBMS。如果发现性能问题，可使用`SHOW PROCESSLIST;`显示所有活动进程（以及它们的线程 ID 和执行时间）然后用`KILL`命令终结某个特定的进程
5. 编写 SELEC 语句的方式不止一种，可以每个都试一下，比较性能
6. 使用 EXPLAIN 语句让 MySQL 解释它将如何执行一条 SELECT 语句。

   ```sql
   mysql> EXPLAIN SELECT * FROM ordertotals;
   +----+-------------+-------------+------------+------+---------------+------+---------+------+------+----------+-------+
   | id | select_type | table       | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
   +----+-------------+-------------+------------+------+---------------+------+---------+------+------+----------+-------+
   |  1 | SIMPLE      | ordertotals | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    6 |   100.00 | NULL  |
   +----+-------------+-------------+------------+------+---------------+------+---------+------+------+----------+-------+
   1 row in set, 1 warning (0.00 sec)
   ```

7. 存储过程效率高
8. 使用正确的数据类型
9. 不要用 SELECT \*
10. 有的操作（包括 INSERT）支持一个可选的 `DELAYED`关键字，如果使用它，将把控制立即返回给调用程序，并且一旦有可能就实际执行该操作。
11. 在导入数据时，应该关闭自动提交。你可能还想删除索引（包括 FULLTEXT 索引），然后在导入完成后再重建它们。
12. 必须索引数据库表以改善数据检索的性能
13. 通过使用多条 SELECT 语句和连接它们的`UNION语句`替换`复杂的OR条件`可以提升效率
14. 如果你有一些表，它们收集数据且不经常被搜索，则在有必要之前不要索引它们。（索引可根据需要添加和删除。）
15. LIKE 很慢。一般来说，最好是使用 FULLTEXT 而不是 LIKE。
