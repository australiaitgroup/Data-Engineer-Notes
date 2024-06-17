# SQL Part 1

## 目录

- [SQL 基础介绍](#sql-基础介绍)
  - [数据类型在 Snowflake 中的分类](#数据类型在-snowflake-中的分类)
  - [各种类型的 Join 操作](#各种类型的-join-操作)
  - [SQL 语句的基本结构](#sql-语句的基本结构)
  - [SQL 操作符](#sql-操作符)
  - [Aggregation Functions](#aggregation-functions)
  - [Numeric Functions](#numeric-functions)
  - [String Functions](#string-functions)
  - [多表查询操作](#多表查询操作)
  - [子查询语句嵌套](#子查询语句嵌套)
- [SQL 案例 1 讲解](#sql-案例-1-讲解)
- [总结](#总结)

## SQL 基础介绍

### 数据类型在 Snowflake 中的分类

在 Snowflake 中，数据类型主要分为以下几类：

1. **数字类型 (Numeric Types)**

   - **NUMBER (DECIMAL)**: 通常用来替代所有的数字类型。DECIMAL (M, D) 中，M 表示精度，D 表示小数位数。例如，DECIMAL(10,2) 表示最多有 10 位数字，其中包含 2 位小数。
   - 其他常见的数字类型包括 INTEGER, FLOAT 等。

2. **字符串类型 (String Types) / 二进制类型 (Binary Types)**

   - **VARCHAR (STRING, TEXT)**: 这是一个变长字符串类型，支持最大 16MB（未压缩）。这种类型的长度是灵活的，适用于各种字符串数据。
   - **BINARY**: 用于存储二进制数据。

3. **逻辑数据类型 (Logical Types)**

   - **BOOLEAN**: 布尔类型，包括 TRUE, FALSE 和 UNKNOWN。

4. **日期和时间类型 (Date & Time Types)**

   - **DATE**: 仅表示日期。
   - **TIME**: 仅表示时间。
   - **DATETIME (TIMESTAMP_NTZ)**: 不带时区的时间戳。
   - **TIMESTAMP_LTZ**: 带本地时区的时间戳（注意时区变化如夏令时）。
   - **TIMESTAMP_TZ**: 带时区的时间戳。

5. **半结构化数据类型 (Semi-structured Data Types)**
   - **VARIANT**: 可以存储 JSON、AVRO、ORC、PARQUET 等半结构化数据。支持最多 16MB（压缩后）。
   - **OBJECT**: 用户定义的结构，可以通过名称访问，可以包含不同的数据类型。
   - **ARRAY**: 基准数学格式，可以通过索引访问，必须包含相同的数据类型。
   - **LATERAL FLATTEN**: 用于解析数组的函数。

### 各种类型的 Join 操作

在 SQL 中，Join 操作用于根据两个或多个表之间的关系从这些表中查询数据。常见的 Join 类型包括：

1. **INNER JOIN**: 仅返回两个表中匹配的行。
2. **LEFT JOIN (LEFT OUTER JOIN)**: 返回左表中的所有行，即使右表中没有匹配项。
3. **RIGHT JOIN (RIGHT OUTER JOIN)**: 返回右表中的所有行，即使左表中没有匹配项。
4. **FULL JOIN (FULL OUTER JOIN)**: 返回两个表中所有行，即使没有匹配项。
5. **NATURAL JOIN**: 自动根据两个表中同名且同类型的列进行匹配。通常不推荐在工业中使用，因为其行为不够明确。

注意：在实际应用中，通常只使用 LEFT JOIN 或 RIGHT JOIN，选择一种方式即可。

### SQL 语句的基本结构

1. **SELECT**: 指定要查询的列。
2. **FROM**: 指定要查询的表。
3. **JOIN**: 用于连接多个表。
4. **WHERE**: 指定筛选条件。
5. **AND/OR**: 组合多个筛选条件。
6. **GROUP BY**: 对查询结果进行分组。
7. **HAVING**: 对分组后的数据进行筛选。
8. **ORDER BY**: 对查询结果进行排序。

### SQL 操作符

1. **数学运算符**: +, -, \*, /, % 等。
2. **比较运算符**: =, <>, <, >, <=, >= 等。
3. **逻辑运算符**: AND, OR, NOT 等。

#### SQL 别名 (Alias)

- 使用别名可以给表或列取一个临时名称，方便书写和阅读。例如：
  ```sql
  SELECT column_name AS alias_name FROM table_name;
  ```

### Aggregation Functions

- 常见的聚合函数包括：COUNT(), SUM(), AVG(), MAX(), MIN() 等。

### Numeric Functions

- 常见的数值函数包括：ABS(), CEIL(), FLOOR(), ROUND(), SQRT() 等。

### String Functions

- 常见的字符串函数包括：CONCAT(), LENGTH(), SUBSTRING(), UPPER(), LOWER() 等。

### 多表查询操作

- **UNION** vs **UNION ALL**:
  - **UNION**: 合并两个查询的结果，并移除重复的记录。
  - **UNION ALL**: 合并两个查询的结果，不移除重复的记录。
  - 建议使用 UNION ALL，因为 UNION 会移除重复记录并消耗更多资源。

### 子查询语句嵌套

- 子查询(Nested Query)是一个嵌套在另一个查询中的查询，用于复杂的数据查询和处理。

### SQL 案例 1 讲解

#### 案例介绍

本案例演示如何使用 SQL 进行数据查询和处理，包括连接数据库、创建数据库和表、插入数据、执行查询等。

#### 具体步骤

1. **连接数据库**

   - 使用适当的数据库连接工具或客户端连接到数据库。

2. **创建数据库和表**

   - 使用 SQL 语句创建数据库和表。

   ```sql
   CREATE DATABASE sample_db;
   CREATE TABLE sample_table (
       id INT PRIMARY KEY,
       name VARCHAR(100),
       age INT
   );
   ```

3. **插入数据**

   - 向表中插入示例数据。

   ```sql
   INSERT INTO sample_table (id, name, age) VALUES (1, 'Alice', 30);
   INSERT INTO sample_table (id, name, age) VALUES (2, 'Bob', 25);
   ```

4. **执行查询**

   - 使用 SELECT 语句查询数据。

   ```sql
   SELECT * FROM sample_table;
   ```

5. **使用 JOIN 操作**

   - 连接多个表并查询数据。

   ```sql
   SELECT a.id, a.name, b.salary
   FROM sample_table a
   INNER JOIN salary_table b ON a.id = b.employee_id;
   ```

6. **使用子查询**

   - 在查询中嵌套另一个查询。

   ```sql
   SELECT name FROM sample_table
   WHERE age = (SELECT MAX(age) FROM sample_table);
   ```

7. **数据汇总和分组**
   - 使用聚合函数对数据进行汇总和分组。
   ```sql
   SELECT age, COUNT(*) FROM sample_table GROUP BY age;
   ```

## 总结

本次课程主要介绍了 SQL 的基本概念和操作，包括数据类型、不同的 JOIN 类型、基本的 SQL 语句结构、常见的 SQL 操作符和函数、多表查询操作以及子查询语句嵌套等内容。通过这些知识点的学习，学生可以更好地掌握 SQL 的基础知识，并在实际项目中应用这些技能。
