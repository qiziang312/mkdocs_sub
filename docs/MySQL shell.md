# MySQL shell简介

MySQL : 数据库

MySQLsh：用来操作数据库的工具

***
# 一、下载

## MySQL下载
**[直接下载](https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-web-community-8.0.46.0.msi)**

## mySQL shell下载
https://dev.mysql.com/get/Downloads/MySQL-Shell/mysql-shell-26.7.1-windows-x86-64bit.msi

下载地址：
`C:\tools\MySQL\MySQL shell`

配置环境变量：
`C:\tools\MySQL\MySQL shell\bin`

***


# 二、mysqlsh基础命令

## 1、打开mysqlsh并链接数据库



##### （1）.本地连接

```bash
mysqlsh -u root -p
```

`mysqlsh` = 打开 MySQL Shell
`-u root` = 使用用户名 "root"（root 是管理员账号）
 `-p` = 需要输入密码
 
 ```shell
 root                      #输入密码
 ```



##### （2）.远程连接

数据库在电脑 `192.168.1.100` 上
用户名是 `admin`
端口是 `3306`（MySQL 默认端口）

```bash
mysqlsh -u admin -h 192.168.1.100 -P 3306 -p
```

`-u admin` = 用户名是 admin
`-h 192.168.1.100` = 数据库在 192.168.1.100 这台电脑上
`-P 3306` = 使用 3306 端口（可以省略，因为这是默认的）
 `-p` = 需要输入密码
 
 ```shell
 root                      #输入密码
 ```



##### （3）.URI 连接

URI 就像网址一样，把信息写在一条里：

```bash
# 连接本地
mysqlsh root:mypassword@localhost

# 连接远程
mysqlsh admin:pass123@192.168.1.100:3306
```


## 2、切换语言

**三种模式：**

|模式|提示符|用途|
|---|---|---|
|SQL|`mysql-sql>`|最常用，用来查数据、改数据|
|JS|`mysql-js>`|写程序用的（JavaScript）|
|PY|`mysql-py>`|写程序用的（Python）|

**怎么切换：**

```sql

\sql     -- 切换到 SQL 模式（最常用）
\js      -- 切换到 JS 模式
\py      -- 切换到 Python 模式
```
💡 **新手建议**：先只用 SQL 模式，看到 `mysql-sql>` 就对了。

#### 基本准则

| 规则           | 说明                              |
| ------------ | ------------------------------- |
| 每条语句以 `;` 结尾 | 不加分号回车 → 进入续行提示符 `->`           |
| `->` 是续行提示   | 上一条语句没写完；补上 `;` 回车即可执行          |
| `\c` 回车      | 放弃当前未写完的命令，清空输入                 |
| 关键字大小写无关     | `SELECT` = `select`，推荐关键字大写便于阅读 |
| 字符串常量使用单引号   | `'张三'`；MySQL 虽然兼容双引号，但标准写法用单引号  |

***

# 三、数据库操作（DDL）



**经上述操作我们就可以在次学习数据库了**

前置条件：mysqlsh中已经链接数据库，如下
```
 MySQL  localhost:3306 ssl  SQL >
```



## 1.数据库操作

```SQL
-- 1.查看所有数据库
SHOW DATABASES;

-- 2.创建数据库
CREATE DATABASE IF NOT EXISTS testdb DEFAULT CHARACTER SET utf8mb4;

-- 3.切换(使用)数据库
USE testdb;

-- 4.删除数据库
DROP DATABASE IF EXISTS testdb;
```



## 2.表相关

### create 创建表

```SQL
CREATE TABLE IF NOT EXISTS student (
    id INT,
    name VARCHAR(20),
    age TINYINT,
    score FLOAT
);
```

### desc 查看表结构

```SQL
DESC student;
```

### drop 删除表

```sql
DROP TABLE IF EXISTS stu;
```

### RENAME TO    表名重命名

```sql
ALTER TABLE student RENAME TO stu;
```

### ADD COLUMN    增加列

```sql
ALTER TABLE stu ADD COLUMN gender CHAR(1);
```

### MODIFY COLUMN    改变列的类型

```sql
ALTER TABLE stu MODIFY COLUMN gender VARCHAR(2);
```

### CHANGE COLUMN    改变列的类型

```sql
ALTER TABLE stu CHANGE COLUMN gender sex CHAR(2);
```

### DROP COLUMN    删除列

```sql
ALTER TABLE stu DROP COLUMN sex;
```

### DEFAULT    默认值约束

```sql
CREATE TABLE 表名 (
    列名 数据类型 DEFAULT 默认值
);
-- 添加/修改默认值
ALTER TABLE student
ALTER COLUMN age SET DEFAULT 20;

-- 删除默认值
ALTER TABLE student
ALTER COLUMN age DROP DEFAULT;
```



## 3.常用数据类型汇总

#### 整数类型

| 类型       | 字节  | 有符号范围                                      | 无符号范围                    | 特点           |
| -------- | --- | ------------------------------------------ | ------------------------ | ------------ |
| TINYINT  | 1   | -128 ~ 127                                 | 0 ~ 255                  | 小整数，年龄、状态标记  |
| SMALLINT | 2   | -32768 ~ 32767                             | 0 ~ 65535                | 短整数          |
| INT      | 4   | -2147483648 ~ 2147483647                   | 0 ~ 4294967295           | 普通整数，id 主键常用 |
| BIGINT   | 8   | -9223372036854775808 ~ 9223372036854775807 | 0 ~ 18446744073709551615 | 大整数，超大编号     |

#### 浮点 / 定点类型

| 类型           | 字节  | 说明                | 特点                                          |
| ------------ | --- | ----------------- | ------------------------------------------- |
| FLOAT        | 4   | 单精度浮点数            | 存在精度丢失，**禁止存金额**                            |
| DOUBLE       | 8   | 双精度浮点数            | 精度高于 float，仍有精度误差                           |
| DECIMAL(m,d) | 可变  | 定点小数；m 总位数，d 小数位数 | **金额必须使用**，例：`DECIMAL(10,2)`总 10 位，保留 2 位小数 |

#### 字符串类型

| 类型         | 最大长度       | 特点                 | 适用场景     |
| ---------- | ---------- | ------------------ | -------- |
| CHAR(n)    | n 最大 255   | 定长，不足补空格，查询速度快     | 身份证、固定编码 |
| VARCHAR(n) | n 最大 65535 | 可变长度，只占实际数据空间，节省存储 | 姓名、地址、备注 |

#### 时间日期类型

|类型|格式|说明|
|---|---|---|
|DATE|yyyy‑mm‑dd|只保存年月日，例：`2026‑09‑05`|
|TIME|hh:mm:ss|只保存时分秒|
|DATETIME|yyyy‑mm‑dd hh:mm:ss|年月日 + 时分秒，例：`2026‑09‑05 14:30:00`|
|TIMESTAMP|yyyy‑mm‑dd hh:mm:ss|格式同 DATETIME；时区自动转换，范围小，可自动更新时间|





## 4.建表 + alter 修改表完整示例

```sql
-- 1. 先创建数据库并切换
CREATE DATABASE IF NOT EXISTS testdb DEFAULT CHARACTER SET utf8mb4;

USE testdb;

-- 2. 建表示例，覆盖多种数据类型
CREATE TABLE IF NOT EXISTS student (
    id INT,
    age TINYINT,
    score DECIMAL(5,2),
    name VARCHAR(20),
    id_card CHAR(18),
    birth DATE,
    create_time DATETIME
);

-- 查看表结构
DESC student;

-- 1. 修改表名 RENAME TO
ALTER TABLE student RENAME TO stu;

-- 2. 添加列 ADD COLUMN，新增一行
ALTER TABLE stu ADD COLUMN gender CHAR(1);

-- 3. 修改字段类型 MODIFY COLUMN
-- ✔只能改类型、长度，不能修改字段名
ALTER TABLE stu MODIFY COLUMN gender VARCHAR(2);

-- 4. 修改字段名+类型 CHANGE COLUMN
-- ✔改名必须用 CHANGE，旧字段名 新字段名 新类型
ALTER TABLE stu CHANGE COLUMN gender sex CHAR(2);

-- 5. 删除列 DROP COLUMN
ALTER TABLE stu DROP COLUMN sex;

-- 删除整张表
DROP TABLE IF EXISTS stu;
```

---

### 速查小表

表格

|功能|语句|
|---|---|
|查看所有库|`SHOW DATABASES;`|
|建库|`CREATE DATABASE IF NOT EXISTS 库名;`|
|切换库|`USE 库名;`|
|删库|`DROP DATABASE IF EXISTS 库名;`|
|建表|`CREATE TABLE 表名(字段 类型,...);`|
|查看表结构|`DESC 表名;`|
|改表名|`ALTER TABLE 旧名 RENAME TO 新名;`|
|新增列|`ALTER TABLE 表 ADD COLUMN 名 类型;`|
|修改字段类型|`ALTER TABLE 表 MODIFY COLUMN 名 新类型;`|
|修改字段名|`ALTER TABLE 表 CHANGE COLUMN 旧名 新名 类型;`|
|删除列|`ALTER TABLE 表 DROP COLUMN 列名;`|
|删除表|`DROP TABLE IF EXISTS 表名;`|

> 易错点：
> 
> 1. **modify 只能改类型，不能改字段名**；改名要用 `CHANGE COLUMN`
> 2. `RENAME TO` 是改**表名**，不是改列名
> 3. drop 可以删库、删表、删列，操作谨慎

## 5.增删改查

### 增  INSERT    插入数据

```sql
-- 格式1：指定列名插入（推荐，列顺序可自由排列）
INSERT INTO 表名 (列1, 列2, 列3, ...)
VALUES (值1, 值2, 值3, ...);

-- 格式2：省略列名（必须按建表时的列顺序给全所有值）
INSERT INTO 表名
VALUES (值1, 值2, 值3, ...);

-- 格式3：一次插入多行
INSERT INTO 表名 (列1, 列2)
VALUES (值1, 值2),
       (值3, 值4),
       (值5, 值6);
```

---

### 查 SELECT 查询数据

```sql
SELECT 列1, 列2, ...       -- 要查哪些列
-- * 代表所有列
FROM 表名                  -- 从哪张表查
WHERE 条件                 -- 筛选行（可选）例：WHERE u_age>18;
ORDER BY 列名 [ASC|DESC]   -- 排序（可选）ASC:升序  DESC：降序
LIMIT 数量;                -- 限制返回行数（可选）
```

### 查 SELECT * 查询所有列

```sql
SELECT * FROM student;
```

### 查 SELECT  DISTINCT 查询所有不重复的值

```sql
SELECT DISTINCT gender FROM student;
```

### 查 FROM 条件查询

```sql
SELECT * FROM student WHERE age > 18;
```

### 查 ORDER BY 升序或降序          ASC:升序  DESC：降序

```sql
SELECT * FROM student ORDER BY age DESC;
```

### 查 LIMIT 跳过行数, 获取行数

```sql
-- 获取前5条记录 SELECT * FROM user LIMIT 5;
SELECT * FROM user LIMIT 5;


-- 跳过2条，取3条：拿第3、4、5行
SELECT * FROM user LIMIT 2, 3;
```

### 查 聚合函数（统计查询）

```sql
--统计行数 
SELECT COUNT(*) FROM student; 

--分数求和 
SELECT SUM(score) FROM student; 

--求分数平均值 
SELECT AVG(score) FROM student; 

--查询分数最大值 
SELECT MAX(score) FROM student; 

--查询分数最小值 
SELECT MIN(score) FROM student; 

--全部聚合函数一起查询 
SELECT COUNT(*), --总人数 
SUM(score), --总分 
AVG(score), --平均分 
MAX(score), --最高分 
MIN(score) --最低分 
FROM student;
```

### 查 GROUP BY 分组 HAVING 分组后过滤

**GROUP BY：按指定列分组，配合聚合函数统计** 
**HAVING：过滤分组后的结果，可以使用聚合函数**

```sql
SELECT gender,COUNT(*) FROM student GROUP BY gender;
```

```sql
SELECT gender,COUNT(*) FROM student GROUP BY gender HAVING COUNT(*) > 2;
```

```sql
SELECT gender,COUNT(*) FROM student WHERE age>18 GROUP BY gender HAVING COUNT(*)>2;
```


---

### 改 UPDATE

```sql
UPDATE 表名
SET 列1 = 新值1,
    列2 = 新值2,
    ...
WHERE 条件;   -- 非常重要！不写 WHERE 会改全表！
```

例：

```sql
-- 把 id=1 的学生年龄改成 21
UPDATE student
SET age = 21
WHERE id = 1;

-- 同时修改多列
UPDATE student
SET age = 22, gender = '男'
WHERE id = 2;

-- 基于原值修改（年龄全部 +1）
UPDATE student
SET age = age + 1;
-- ⚠️ 没有 WHERE，全表所有行的 age 都会 +1，谨慎使用！
```


**！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！**
**！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！**
**！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！** **⚠️ 致命提醒**：UPDATE 不写 WHERE 会更新**整张表**的所有行！执行前务必确认 WHERE 条件。
**！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！**
**！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！**
**！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！**

***

### 删DELETE  删除数据 


```sql
DELETE FROM 表名
WHERE 条件;   -- 同样非常重要！
```

例：

```sql
-- 删除 id=4 的学生
DELETE FROM student
WHERE id = 4;

-- 删除年龄小于18的学生
DELETE FROM student
WHERE age < 18;

-- 删除全部数据（保留表结构）
DELETE FROM student;
-- 或更快的方式（不可回滚）：
TRUNCATE TABLE student;
```

**！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！**
**！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！**
**！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！** **⚠️ 致命提醒**：DELETE 不写 WHERE 会更新**整张表**的所有行！执行前务必确认 WHERE 条件。
**！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！**
**！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！**
**！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！**

> **DELETE vs TRUNCATE vs DROP 区别**：
> 
> - `DELETE`：删除行，可带 WHERE，可回滚，慢
> - `TRUNCATE`：清空全表，不可带 WHERE，不可回滚，快
> - `DROP`：删除整张表（连结构一起删）

---


## 6.键位讲解（约束与键）

SQL 中的 "键" 是用来**保证数据完整性和唯一性**的约束。

### 1. 主键（PRIMARY KEY）

- 唯一标识表中的每一行
- **不能重复，不能为 NULL**
- 一张表只能有一个主键

```SQL
-- 建表时指定
CREATE TABLE student (
    id   INT PRIMARY KEY,   -- 方式1：列级定义
    name VARCHAR(50)
);

-- 方式2：表级定义（适合联合主键）
CREATE TABLE score (
    student_id INT,
    course_id  INT,
    score      INT,
    PRIMARY KEY (student_id, course_id)  -- 两列联合做主键
);

-- 自增主键（最常用）
CREATE TABLE student (
    id   INT PRIMARY KEY AUTO_INCREMENT,  -- 自动从1开始递增
    name VARCHAR(50)
);
```

### 2. 外键（FOREIGN KEY）

- 引用另一张表的主键，建立两表之间的关联
- 保证**参照完整性**：外键的值必须在被引用表的主键中存在，或为 NULL

```SQL
-- 班级表（被引用）
CREATE TABLE class (
    class_id   INT PRIMARY KEY,
    class_name VARCHAR(50)
);

-- 学生表（引用班级表）
CREATE TABLE student (
    id        INT PRIMARY KEY,
    name      VARCHAR(50),
    class_id  INT,
    FOREIGN KEY (class_id) REFERENCES class(class_id)
);
```

> 外键约束意味着：student 表的 class_id 只能填 class 表中已存在的 class_id，否则报错。

### 3. 唯一键（UNIQUE）

- 保证某列的值**不重复**
- 和主键的区别：唯一键**可以为 NULL**（且允许多个 NULL），一张表可以有多个唯一键

```SQL
CREATE TABLE student (
    id    INT PRIMARY KEY,
    name  VARCHAR(50),
    phone VARCHAR(20) UNIQUE   -- 手机号不能重复
);
```

### 4. 非空约束（NOT NULL）

- 保证某列**不能为 NULL**

```SQL
CREATE TABLE student (
    id   INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL   -- 姓名不能为空
);
```

### 5. 检查约束（CHECK）

- 限制某列的取值范围

```SQL
CREATE TABLE student (
    id   INT PRIMARY KEY,
    name VARCHAR(50),
    age  INT CHECK (age >= 0 AND age <= 150),  -- 年龄必须在0~150之间
    gender VARCHAR(10) CHECK (gender IN ('男', '女'))
);
```

### 键约束速查表

表格

|约束类型|关键字|能否重复|能否为 NULL|每表数量|
|---|---|---|---|---|
|主键|PRIMARY KEY|❌|❌|最多 1 个|
|外键|FOREIGN KEY|✅|✅|多个|
|唯一键|UNIQUE|❌|✅|多个|
|非空|NOT NULL|✅|❌|多个|
|检查|CHECK|—|—|多个|
|默认值|DEFAULT|—|—|多个|

---




## 7.子查询
子查询：`()`包裹嵌套 SELECT，分 4 类

### **标量子查询**：返回单个值，`= > <`

```sql
SELECT * FROM emp WHERE salary > (SELECT AVG(salary) FROM emp);
```

### **列子查询**：返回一列多行，`IN / ANY / ALL`

```sql
SELECT * FROM emp WHERE dept_id IN (SELECT id FROM dept);
```

### **表子查询**：FROM 后，**必须别名**，作为临时表

```sql
SELECT * FROM (SELECT * FROM emp) t;
```

###  **相关子查询**：引用外层表字段，效率差

**EXISTS / NOT EXISTS**：判断有无结果，大数据优于 IN

```sql
SELECT * FROM dept d WHERE EXISTS(SELECT 1 FROM emp e WHERE e.dept_id=d.id);
```

## 8.表关联

业务拆分多张表，依靠**关联字段**拼接多表查询数据。

**emp 员工表**

|id|name|salary|dept_id|
|---|---|---|---|
|1|张三|5000|10|
|2|李四|6000|20|
|3|王五|4500|NULL|

**dept 部门表**

|id|dept_name|
|---|---|
|10|研发部|
|20|市场部|
|30|财务部|

关联字段：`emp.dept_id = dept.id`


### **内连接 INNER JOIN** 只返回两边匹配的数据

```sql
SELECT * FROM emp e INNER JOIN dept d ON e.dept_id=d.id;
```

### **左连接 LEFT JOIN** 左表全部保留，右表匹配不到为`NULL`

```sql
SELECT * FROM emp e LEFT JOIN dept d ON e.dept_id=d.id;
```

**右连接 RIGHT JOIN** 右表全部保留，左表匹配不到为`NULL`
**全连接**：MySQL 不支持，用`UNION`合并左 + 右连接实现

> `ON`写关联条件；`WHERE`做结果过滤

### 自连接

一张表自己关联自己，起不同别名

```sql
SELECT a.name,b.name FROM emp a JOIN emp b ON a.mgr = b.id;
```

### UNION / UNION ALL

- `UNION`：合并结果集，**自动去重**
- `UNION ALL`：直接拼接，不去重，速度更快


## 8.索引

### 建表加索引

```sql
CREATE TABLE emp (
  id INT PRIMARY KEY,  -- 主键索引
  name VARCHAR(20),
  dept_id INT
);

INSERT INTO emp VALUES
(1,'张三',10),
(2,'李四',20),
(3,'王五',10),
(4,'赵六',20);
```

###  给 dept_id 创建索引

```sql
CREATE INDEX idx_dept ON emp(dept_id);
```

### 2. 查看索引

```sql
SHOW INDEX FROM emp;
```

|Table|Key_name|Column_name|
|---|---|---|
|emp|PRIMARY|id|
|emp|idx_dept|dept_id|

> PRIMARY 就是主键索引；idx_dept 是我们新建的索引。

### 3. 使用索引查询

```sql
SELECT * FROM emp WHERE dept_id = 10;
```

**执行结果**

|id|name|dept_id|
|---|---|---|
|1|张三|10|
|3|王五|10|

### EXPLAIN 看有没有用到索引

```sql
EXPLAIN SELECT * FROM emp WHERE dept_id = 10;
```

> 输出里面 `key` 这一列的值是 `idx_dept` → **成功用上索引**

### 5. 索引失效演示（给索引列做运算）

```sql
EXPLAIN SELECT * FROM emp WHERE dept_id + 0 = 10;
```

> `key` 为 NULL，**索引失效，不走索引**

### 6. 删除索引

```sql
DROP INDEX idx_dept ON emp;
```

## 9.视图

**保存好的一条 SELECT 查询语句**，虚拟表，本身不存真实数据。

每次查询视图，就会执行内部封装的 SQL。

### 测试表沿用 emp

```sql
CREATE TABLE emp (
  id INT PRIMARY KEY,
  name VARCHAR(20),
  dept_id INT,
  salary INT
);
INSERT INTO emp VALUES
(1,'张三',10,5000),
(2,'李四',20,6000),
(3,'王五',10,4500),
(4,'赵六',20,7000);
```

### 1. 创建视图

```sql
-- 创建视图：只查询10号部门员工
CREATE VIEW v_emp_10 AS
SELECT id,name,salary FROM emp WHERE dept_id = 10;
```

### 2. 使用视图，像查表一样

```sql
SELECT * FROM v_emp_10;
```

查询结果：

|id|name|salary|
|---|---|---|
|1|张三|5000|
|3|王五|4500|

### 3. 查看视图

```sql
SHOW CREATE VIEW v_emp_10;
```

### 4. 修改视图

```sql
ALTER VIEW v_emp_10 AS
SELECT id,name,salary,dept_id FROM emp WHERE dept_id=10;
```

### 5. 删除视图

```sql
DROP VIEW IF EXISTS v_emp_10;
```

