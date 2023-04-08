

# Mysql基本操作

## 全局操作

### 查询Mysql服务

"此电脑->管理->服务"找到Mysql

<img src="C:\Users\谢功庆\AppData\Roaming\Typora\typora-user-images\image-20230407215459308.png" alt="image-20230407215459308" style="zoom: 50%;" />

也可以打开cmd,输入“services.msc”

<img src="C:\Users\谢功庆\AppData\Roaming\Typora\typora-user-images\image-20230407215942037.png" alt="image-20230407215942037" style="zoom:50%;" />

### 查看配置文件

打开MySQL的配置文件`my.ini`，一般位于MySQL的安装目录下

C:\Program Files\MySQL\MySQL Server 8.0\my.ini

可以查看和修改里面的配置，在配置文件修改的配置是永久的，不过新的配置需要重启Mysql服务后才能生效。

### 启动和关闭服务

用管理员身份打开cmd命令窗口(必须是以关闭员身份，否则操作失败)

启动服务命令：net start mysql80(以服务名为准)

关闭服务命令：net stop mysql80

![image-20230407221729725](C:\Users\谢功庆\AppData\Roaming\Typora\typora-user-images\image-20230407221729725.png)

### 连接（登录）Mysql

 cmd命令：`mysql -uroot -p` (这里一定是mysql,root用户，-p是密码)

### 全局配置

#### 全局配置查看命令（需要先登录MySQL）

查看所有全局配置：`SHOW VARIABLES`;

查看指定配置信息：`SHOW VARIABLES LIKE '%setting%'`//setting为指定配置

这里我们以时区为例，查看、修改

`show variables like'%time_zone'`;//查看时区

 `set global time_zone = '+8:00'`;//修改时区，不过并非永久的

在配置文件my.ini里修改时区为东八区，这样才是永久的

`default_time_zone = '+8:00'`

（更多全局配置->百度，chat)

## Database操作

### 增

- 创建数据库

  创建数据库的基本语法：CREATE DATABASE  dB_name;	但是每次创建的时候最好判断一下是否已有这个数据库

  所以语句改为：`CREATE DATABASE IF NOT EXISTS db_name;``

  如果要顺便设置字符集可以在后面：`CREATE DATABASE IF NOT EXISTS db_name DEFAULT CHARACTER SET UTF8;`

  

- 使用数据库

  USE db_name;切换数据库

### 删

【小伙子，删库跑路是要坐牢的，不要太皮】

​				除非极特殊情况，否则禁止使用该命令：`DROP DATABASE db_name;`



### 改

- 修改库的配置

  自行百度

### 查

- 显示数据库的信息

  使用以下 SQL 语句查看数据库的大小、版本、字符集等信息

  - ```mysql
    SUM(data_length + index_length) / 1024 / 1024 AS `Size (MB)`, 
    
    VERSION() AS `Server Version`,   
    
    DEFAULT_CHARACTER_SET_NAME AS `Character Set`,  
    
    DEFAULT_COLLATION_NAME AS `Collation`
    
    - 
    ```

- 查看数据库

  展示所有数据库：SHOW DATABASES;

  查看正在使用的数据库：SELECT DATABASE();

  查看创建数据库的命令（此命令可以查看到数据库的一些配置）：SHOW CREATE DATABASE db_Name;

## 表的操作

### 增

- 创建表

  CREATE TABLE IF NOT EXISTS table_Name(col1 type,col2 type,col3 type...);

  

- 添加字段值

  INSECT INTO table_name(col1,col2,col3...)//如果是所有字段，那么直接写表名即可

  values (value1,value2,value3...);

  如果是添加多行那么：

  values

  (value1,value2,value3...)，

  (value1,value2,value3...)，

  .....

  

- 设置表

  可以在创建表的时候添加约束条件，例如添加主键 primary key  外键 foreign key 唯一值 unique，非空值：nounull，

   检查值 check（但是Mysql这个没用，不能检查，也就是说实际上没用），默认值 default

  也可以在创建完表后在设置约束条件，使用ALTER TABLE语句在已经创建的表上添加约束条件。语法格式如下：

  ```MySQL
  1.主键约束
  添加:alter table  table_name add primary key (字段)
  添加B:alter table table_name modify 列名 数据类型 primary key;
  删除:alter table table_name drop primary key
  2.非空约束
  添加:alter  table table_name modify 列名 数据类型  not null 
  删除:alter table table_name modify 列名 数据类型 null
  3.唯一约束
  添加:alter table table_name add unique 约束名（字段）-- 同上，也有方式B
  删除:alter table table_name drop key 约束名
  4.自动增长
  添加:alter table table_name  modify 列名 int  auto_increment
  删除:alter table table_name modify 列名 int  
  5.外键约束
  添加:alter table table_name add constraint 约束名 foreign key(外键列) 
  references 主键表（主键列）
  删除:
  第一步:删除外键
  alter table table_name drop foreign key 约束名
  第二步:删除索引
  alter  table table_name drop  index 索引名
  [^1]: 
  约束名和索引名一样
  6.默认值
  添加:alter table table_name alter 列名  set default '值'
  删除:alter table table_name alter 列名  drop default
  
  ```

  

### 删

- 删表

  `drop table if exists table_name;`

- 删除表的所有信息

  `truncate table table_name;`//注意这个操作不能回滚，要想可以回滚用delete（推荐）

  `delete from table_name;`

- 删除指定字段

  在delete后面加where语句判断即可

  `delete from table_name where expression;`

### 改

- 修改表名

  `ALTER TABLE old_Name RENAME new_Name;`

  

- 修改表的信息

  

  1. 修改表结构 我们可以使用 `ALTER TABLE` 语句修改表结构，常见的操作包括添加、删除、修改列、修改列属性等。例如，我们可以添加一列 `age` 到表 `student` 中：

  ```sql
  ALTER TABLE student
  ADD COLUMN age INT(11) AFTER name;
  ```

  上述语句将在表 `student` 中添加一列 `age`，数据类型为 `INT`，长度为 `11`，插入在 `name` 列之后。

  

   2. 修改表数据 我们可以使用 `UPDATE` 语句修改表中的数据，常见的操作包括修改单行数据、批量修改数据等。例如，我们可以将表 `student` 中 `id` 为 `1` 的学生的年龄修改为 `18`：

  ```sql
  UPDATE student
  SET age = 18
  WHERE id = 1;
  ```

  上述语句将修改表 `student` 中 `id` 为 `1` 的学生的年龄为 `18`。

  

   3. 修改表名 我们可以使用 `RENAME TABLE` 语句修改表名。例如，我们可以将表 `student` 修改为 `user`：

  ```sql
  RENAME TABLE student TO user;
  ```

  上述语句将修改表 `student` 的名字为 `user`。 

  

  4. 修改表的存储引擎 我们可以使用 `ALTER TABLE` 语句修改表的存储引擎。例如，我们可以将表 `user` 的存储引擎从 MyISAM 修改为 InnoDB：

  ```mysql
  ALTER TABLE user
  ENGINE=InnoDB;
  ```

### 查

- 单表综合查询

  下面是一个单表的综合查询案例，包含了多种查询方式和 SQL 关键字，旨在尽可能地复杂： 假设我们有一个 `books` 表，包含以下列：`id`、`title`、`author`、`price`、`publish_date`、`publisher`、`category`。我们需要查询出 `price` 最高的 5 本书，并按照 `publish_date` 的升序排序。同时，我们需要计算出这 5 本书的平均价格和总价格，并按照 `category` 进行分组统计。 查询语句如下：

  ```mysql
  SELECT 
    id, 
    title, 
    author, 
    price, 
    publish_date, 
    publisher, 
    category 
  FROM 
    books
  WHERE 
    price IN (
      SELECT 
        price 
      FROM 
        books 
      ORDER BY 
        price DESC
      LIMIT 
        5
    ) 
  ORDER BY 
    publish_date ASC 
  GROUP BY 
    category 
  WITH ROLLUP;
  ```

  上述查询语句将返回 5 本价格最高的书籍，并按照 `publish_date` 的升序排序。同时，它还计算出了这 5 本书的平均价格和总价格，并按照 `category` 进行了分组统计。最后，使用 `WITH ROLLUP` 将这些统计结果进行了一次汇总。

  综合查询语句的执行顺序是：

  > > 1. ```mysql
  > >    FROM：查询语句中的 FROM 关键字指定要查询的表。
  > >    WHERE：查询语句中的 WHERE 关键字指定查询的条件。
  > >    GROUP BY：查询语句中的 GROUP BY 关键字按照指定的列进行分组统计。
  > >    HAVING：查询语句中的 HAVING 关键字对分组结果进行过滤。
  > >    SELECT：查询语句中的 SELECT 关键字选择要查询的列，可以使用聚合函数计算结果。
  > >    DISTINCT：查询语句中的 DISTINCT 关键字去除查询结果中的重复值。
  > >    ORDER BY：查询语句中的 ORDER BY 关键字指定查询结果的排序方式。
  > >    LIMIT：查询语句中的 LIMIT 关键字限制查询结果的数量。 
  > >    -- 根据上述执行顺序，可以得出以下代码的执行顺序：
  > >    -- 执行子查询，查询出价格最高的前 5 本书的价格。在 books 表中，WHERE 子句筛选出价格在子查询中查询出的价格范围内的书籍。根据 publish_date 列进行升序排序。按照 category 分组统计。计算每个分组的平均价格、总价格。使用 WITH ROLLUP 在分组统计的基础上汇总统计。选择 id、title、author、price、publish_date、publisher、category 列。
  > >    -- 返回查询结果。 注意，实际执行顺序可能会受到数据库优化器的影响，因此不一定完全按照上述顺序执行。但是，了解 SQL 语句的执行顺序可以帮助我们更好地理解和优化查询语句。
  > >    ```

- 多表查询

	- 案例
	
	  假设有两张表，一张是顾客信息表（customers），另一张是订单信息表（orders）。它们的结构如下所示：
	
	  ```mysql
	  - CREATE TABLE customers (
	        customer_id INT PRIMARY KEY,
	        name VARCHAR(50),
	        email VARCHAR(50),
	        phone VARCHAR(20)
	    );
	    CREATE TABLE orders (
	        order_id INT PRIMARY KEY,
	        customer_id INT,
	        order_date DATE,
	        total_amount DECIMAL(10, 2),
	        FOREIGN KEY (customer_id) REFERENCES customers (customer_id)
	    );
	  ```
	
	  其中，customers表存储了顾客的基本信息，包括customer_id、name、email和phone四个字段；orders表存储了订单的信息，包括order_id、customer_id、order_date和total_amount四个字段，其中customer_id为外键，关联到customers表的customer_id字段。 现在我们需要查询所有订单的详细信息及对应的顾客信息，包括订单号、订单日期、订单总金额、顾客姓名、顾客邮箱和顾客电话。这个查询可以使用内联、外联和自联三种方式来实现，具体如下所示
	
	  
	
	  ##### 内联
	
	  1. 内联查询是指将两张表的所有匹配行连结起来，只返回符合条件的数据行。内联查询可以使用INNER JOIN关键字来实现。以下是一个内联查询的示例：
	
	     ```mysql
	     SELECT orders.order_id, orders.order_date, orders.total_amount, customers.name, customers.email, customers.phone
	     FROM orders
	     INNER JOIN customers
	     ON orders.customer_id = customers.customer_id;
	     ```
	
	     这个查询语句中，使用INNER JOIN将orders表和customers表关联起来，关联条件为orders表的customer_id等于customers表的customer_id。查询结果将返回所有匹配的订单信息和对应的顾客信息。
	
	- 外联
	
	  外联查询是指将两张表的所有行（包括不匹配的行）连结起来，返回符合条件的数据行和空值行。外联查询可以使用LEFT JOIN或RIGHT JOIN关键字来实现。以下是一个左外联查询的示例：
	
	  ```mysql
	  SELECT orders.order_id, orders.order_date, orders.total_amount, customers.name, customers.email, customers.phone
	  FROM orders
	  LEFT JOIN customers
	  ON orders.customer_id = customers.customer_id;
	  ```
	
	  这个查询语句中，使用LEFT JOIN将orders表和customers表左外联起来，关联条件为orders表的customer_id等于customers表的customer_id。查询结果将返回所有订单信息和对应的顾客信息，如果订单信息没有对应的顾客信息，则顾客信息的字段将为NULL。
	
	- 自联
	
	  自联查询是指将同一张表的不同行连结起来，用于查询具有父子关系的数据。自联查询可以使用AS关键字来实现。以下是一个自联查询的示例：
	
	  ```mysql
	  SELECT c1.name, c1.email, c1.phone, c2.name, c2.email, c2.phone
	  FROM customers c1, customers c2
	  WHERE c1.customer_id = 1 AND c2.customer_id = 2;
	  ```
	
	  这个查询语句中，将customers表自联两次，分别用c1和c2来表示，通过WHERE子句指定查询条件，即c1的customer_id等于1，c2的customer_id等于2。查询结果将返回两个顾客的姓名、邮箱和电话号码。
	
	  
	
	  ### 其他查询
	

​		除了内联、外联和自联查询之外，MySQL中还有一些其他常用的多表查询方式，包括子查询、UNION查询和交叉连接查询等。		这里再举几个例子：

1. 子查询： 子查询是指将一个查询语句嵌套在另一个查询语句中，用于查询符合某些条件的数据。以下是一个子查询的示例：

```sql
SELECT *
FROM orders
WHERE customer_id IN (
    SELECT customer_id
    FROM customers
    WHERE name LIKE 'J%'
);
```

这个查询语句中，外部查询语句查询orders表中所有顾客姓名以J开头的订单信息，内部查询语句查询customers表中所有姓名以J开头的顾客的customer_id。查询结果将返回所有符合条件的订单信息。 2. UNION查询： UNION查询是指将两个或多个查询语句的结果集合并成一个结果集，用于查询不同的表或相同的表的不同列。以下是一个UNION查询的示例：

```sql
SELECT name, email
FROM customers
UNION
SELECT 'Admin', 'admin@example.com'
FROM dual;
```

这个查询语句中，将customers表中所有顾客的姓名和邮箱查询出来，并与一个包含固定值的查询结果合并。需要注意的是，UNION查询要求两个查询的列数和数据类型必须相同，否则会出现错误。 3. 交叉连接查询： 交叉连接查询是指将两张表的所有行组合起来，返回所有可能的组合结果。交叉连接查询可以使用CROSS JOIN关键字来实现。以下是一个交叉连接查询的示例：

```sql
SELECT customers.name, orders.order_id
FROM customers
CROSS JOIN orders;
```

这个查询语句中，将customers表和orders表进行交叉连接，查询结果将返回所有顾客姓名和订单号的组合情况。需要注意的是，交叉连接查询可能会产生大量的结果行，应该谨慎使用。 总之，MySQL中有多种多表查询方式，开发人员可以根据具体的查询需求选择合适的方式来实现。
