# SQL Part 2

## 目录

- [SQL 基础介绍](#sql-基础介绍)
  - [SELECT & WHERE](#select--where)
  - [GROUP BY & HAVING](#group-by--having)
  - [Nested Query](#nested-query)
  - [Rank/Row Number](#rankrow-number)
  - [Partition By](#partition-by)
  - [Range Selection](#range-selection)
  - [Further Analytic Functions](#further-analytic-functions)
  - [表 (Table) 与视图 (View) 的比较](#表-table-与视图-view-的比较)
  - [SQL Tuning](#sql-tuning)
  - [了解索引 (Index) 的优点，分类，语法](#了解索引-index-的优点分类语法)
  - [窗口分析函数 (Window Analytic Functions)](#窗口分析函数-window-analytic-functions)
  - [窗口分析函数高阶](#窗口分析函数高阶)
  - [Snowflake Query 的 JSON 格式语法讲解](#snowflake-query-的-json-格式语法讲解)
  - [Snowflake Flatten Function to Parse Arrays](#snowflake-flatten-function-to-parse-arrays)
  - [如何用 Command Line 把本地数据导入 Snowflake](#如何用-command-line-把本地数据导入-snowflake)
  - [SQL Cumulative Sum & Moving Average](#sql-cumulative-sum--moving-average)
  - [Common Table Expression (CTE) 实例讲解](#common-table-expression-cte-实例讲解)
  - [NTILE 案例讲解](#ntile-案例讲解)
  - [Recursive CTE: 语法及案例讲解](#recursive-cte-语法及案例讲解)
- [总结](#总结)

## SQL 基础介绍

### SELECT & WHERE

- **SELECT**: 用于从数据库中选择数据。
- **WHERE**: 用于根据指定条件过滤记录。
- 示例：
  ```sql
  SELECT * FROM employees WHERE department = 'Sales';
  ```

### GROUP BY & HAVING

- **GROUP BY**: 用于将相同数据分组。
- **HAVING**: 用于筛选分组后的数据。
- 示例：
  ```sql
  SELECT department, COUNT(*)
  FROM employees
  GROUP BY department
  HAVING COUNT(*) > 10;
  ```

### Nested Query

- 嵌套查询，即在一个查询中嵌套另一个查询。
- 示例：
  ```sql
  SELECT name FROM employees
  WHERE salary > (SELECT AVG(salary) FROM employees);
  ```

### Rank/Row Number

- **RANK**: 行排名，允许有间隔。
  ```sql
  SELECT name, RANK() OVER (ORDER BY salary DESC) AS rank
  FROM employees;
  ```
- **DENSE_RANK**: 行排名，不允许有间隔。
  ```sql
  SELECT name, DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rank
  FROM employees;
  ```
- **ROW_NUMBER**: 每行的唯一编号。
  ```sql
  SELECT name, ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num
  FROM employees;
  ```

### Partition By

- 用于在分区内进行排名、编号等操作。
- 示例：
  ```sql
  SELECT name, department, RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rank
  FROM employees;
  ```

### Range Selection

- 用于选择特定范围内的数据。
- 示例：
  ```sql
  SELECT * FROM employees
  WHERE hire_date BETWEEN '2020-01-01' AND '2021-01-01';
  ```

### Further Analytic Functions

- 高级分析函数，如累计和、移动平均等。
- 示例：
  ```sql
  SELECT name, salary,
         SUM(salary) OVER (ORDER BY hire_date ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS moving_sum
  FROM employees;
  ```

### 表 (Table) 与视图 (View) 的比较

- **表 (Table)**: 物理存在的数据存储结构。
- **视图 (View)**: 逻辑存在的虚拟表，是对查询结果的封装。
- 示例：
  ```sql
  CREATE VIEW sales_view AS
  SELECT * FROM employees WHERE department = 'Sales';
  ```

### SQL Tuning

- 优化 SQL 查询性能的技术。
- 包括索引使用、查询重写等。

#### 了解索引 (Index) 的优点，分类，语法

- **优点**: 加快数据检索速度。
- **分类**: 主键索引、唯一索引、普通索引、全文索引等。
- **语法**:
  ```sql
  CREATE INDEX idx_name ON employees (name);
  ```

### 窗口分析函数 (Window Analytic Functions)

- **DENSE_RANK**: 分区内无间隔排名。
  ```sql
  SELECT name, DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rank
  FROM employees;
  ```
- **RANK**: 分区内有间隔排名。
  ```sql
  SELECT name, RANK() OVER (ORDER BY salary DESC) AS rank
  FROM employees;
  ```
- **ROW_NUMBER**: 分区内行编号。
  ```sql
  SELECT name, ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num
  FROM employees;
  ```

### 窗口分析函数高阶

- **Partition By** 的使用：
  ```sql
  SELECT name, department, RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rank
  FROM employees;
  ```
- **LEAD**: 提取下一行的数据。
  ```sql
  SELECT name, LEAD(salary, 1) OVER (ORDER BY hire_date) AS next_salary
  FROM employees;
  ```
- **LAG**: 提取上一行的数据。
  ```sql
  SELECT name, LAG(salary, 1) OVER (ORDER BY hire_date) AS prev_salary
  FROM employees;
  ```

### Snowflake Query 的 JSON 格式语法讲解

- 使用 Snowflake 查询和处理 JSON 数据的语法。
- 示例：
  ```sql
  SELECT
      json_column:key1::string AS key1_value
  FROM table_name;
  ```

### Snowflake Flatten Function to Parse Arrays

- 使用 Snowflake 的 FLATTEN 函数解析数组。
- 示例：
  ```sql
  SELECT
      value:key1::string AS key1_value
  FROM table_name,
       LATERAL FLATTEN(input => json_column);
  ```

### 如何用 Command Line 把本地数据导入 Snowflake

1. **检查输入源 (如 CSV, TXT) 的数据质量**。
2. **创建目标表的 SQL**。
3. **打开 CLI 或 Web 界面**。
4. **选择数据库和表**。
5. **创建文件格式**。
   ```sql
   CREATE FILE FORMAT my_csv_format
   TYPE = 'CSV'
   FIELD_OPTIONALLY_ENCLOSED_BY = '"';
   ```
6. **创建 Stage**。
   ```sql
   CREATE STAGE my_stage;
   ```
7. **将文件放入 Stage**。
   ```sql
   PUT file://path_to_file/myfile.csv @my_stage;
   ```
8. **将数据复制到表中**。

   ```sql


   COPY INTO my_table
   FROM @my_stage
   FILE_FORMAT = (FORMAT_NAME = 'my_csv_format');
   ```

### SQL Cumulative Sum & Moving Average

- 累计和与移动平均的计算。
- 示例：
  ```sql
  SELECT name,
         SUM(salary) OVER (ORDER BY hire_date ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS moving_sum,
         AVG(salary) OVER (ORDER BY hire_date ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS moving_avg
  FROM employees;
  ```

### Common Table Expression (CTE) 实例讲解

- 使用 CTE 提高查询的可读性。
- 示例：
  ```sql
  WITH sales_dept AS (
      SELECT * FROM employees WHERE department = 'Sales'
  )
  SELECT * FROM sales_dept WHERE salary > 50000;
  ```

### NTILE 案例讲解

- 使用 NTILE 将数据划分为 N 组。
- 示例：
  ```sql
  SELECT name,
         NTILE(4) OVER (ORDER BY salary) AS quartile
  FROM employees;
  ```

### Recursive CTE: 语法及案例讲解

- 递归 CTE，用于处理分层数据。
- 示例：
  ```sql
  WITH RECURSIVE employee_hierarchy AS (
      SELECT emp_name, emp_manager, 1 AS level
      FROM employees
      WHERE emp_manager IS NULL
      UNION ALL
      SELECT e.emp_name, e.emp_manager, eh.level + 1
      FROM employees e
      INNER JOIN employee_hierarchy eh ON e.emp_manager = eh.emp_name
  )
  SELECT * FROM employee_hierarchy;
  ```

## 总结

本次课程深入探讨了 SQL 的高级功能，包括高级分析函数、窗口函数、嵌套查询、索引优化、递归 CTE 等内容。通过这些知识点的学习，学生可以掌握更多高级 SQL 技能，提高数据查询和处理的效率。在实际应用中，这些技能不仅可以帮助解决复杂的业务需求，还可以显著提升数据库的性能。通过系统的练习和实践，学生们将能够在实际项目中熟练应用这些高级 SQL 技能，成为数据处理和分析的专家。
