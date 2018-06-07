---
layout: page
title: SQL
category: note
tags: Others
---

* content
{:toc}


# SQL

## 基础操作

```sql
CREATE DATABASE # 创建新数据库
ALTER DATABASE  # 修改数据库
CREATE TABLE    # 创建新表
ALTER TABLE # 变更（改变）数据库表
DROP TABLE  # 删除表
CREATE INDEX    # 创建索引（搜索键）
DROP INDEX  # 删除索引
```

## Learning SQL

### 启动

```sql
# MacOS
>>sudo mysqld_safe
>>mysql -u root -p # login as root
>>create database bank #create a new database
>>grant all priviledges on bank.* to 'ychen'@'localhost' identified by 'password'; #配置权限
>>quit;
>>mysql -u ychen -p
>>use bank;
```

### 创建表

```mysql
CREATE TABLE person
 (person_id SMALLINT UNSIGNED,
  fname VARCHAR(20),
  lname VARCHAR(20),
  gender ENUM('M','F'),
  birth_date DATE,
  street VARCHAR(30),
  city VARCHAR(20),
  state VARCHAR(20),
  country VARCHAR(20),
  postal_code VARCHAR(20),
  CONSTRAINT pk_person PRIMARY KEY (person_id)
 );

# describe table person
DESC person;

# create another tabel with foreign key
CREATE TABLE favorite_food
 (person_id SMALLINT UNSIGNED,
  food VARCHAR(20),
  CONSTRAINT pk_favorite_food PRIMARY KEY (person_id, food),
  CONSTRAINT fk_fav_food_person_id FOREIGN KEY (person_id)
    REFERENCES person (person_id)
 );
```

#### 修改定义

```mysql
ALTER TABLE person MODIFY person_id SMALLINT UNSIGNED AUTO_INCREMENT;
```

### 插入行

```mysql
INSERT INTO person
 (person_id, fname, lname, gender, birth_date)
VALUES (null, 'William','Turner', 'M', '1972-05-27');
```

### 查找行

```mysql
SELECT person_id, fname, lname, birth_date
FROM person;

SELECT person_id, fname, lname, birth_date
FROM person
WHERE person_id = 1
ORDER BY lname;
```

### 更新行

```mysql
UPDATE person
SET street = '1225 Tremont St.',
  city = 'Boston',
  state = 'MA',
  country = 'USA',
  postal_code = '02138'
WHERE person_id = 1;
```

### 删除行

```mysql
DELETE FROM person
WHERE person_id = 2;
```

### 查看tables

```mysql
SHOW TABLES;
```



## Query Primer

| Clause name | Purpose                                                      |
| ----------- | ------------------------------------------------------------ |
| Select      | Determines which columns to include in the query’s result set |
| From        | Identifies the tables from which to draw data and how the tables should be joined |
| Where       | Filters out unwanted data                                    |
| Group by    | Used to group rows together by common column values          |
| Having      | Filters out unwanted groups                                  |
| Order by    | Sorts the rows of the final result set by one or more columns |

###  The SELECT Clause

things can be included in the SELECT clause

- Literals, such as numbers or strings
- Expressions, such as transaction.amount * −1
- Built-in function calls, such as ROUND(transaction.amount, 2)
- User-defined function calls

```mysql
SELECT emp_id,
  'ACTIVE',
  emp_id * 3.14159,
  UPPER(lname)
FROM employee;

# call functions
SELECT VERSION(),
  USER(),
  DATABASE();
```

#### Column Aliases

给columns命名

```mysql
# 'AS' can be omitted
SELECT emp_id,
  'ACTIVE' (AS) emp_status,
  emp_id*3.14159 (AS) empid_x_pi,
  UPPER(lname) (AS) last_name_upper
FROM employee;
```

#### Select distinct

```mysql
SELECT DISTINCT cust_id
FROM account;
```

### The FROM Clause

Identifies the tables from which to draw data and how the tables should be joined

Tables**:

- Permenant tables: created using the *create table* statement

- Temporary tables: rows returned by a subquery

  ```mysql
  SELECT e.emp_id, e.fname, e.lname
  FROM (SELECT emp_id, fname, lname, start_date, title
        FROM employee) e;
  ```

- Views: created using the *create view* statement

  ```mysql
  CREATE VIEW employee_vw AS
  SELECT emp_id, fname, lname,
    YEAR(start_date) start_year
  FROM employee;

  SELECT emp_id, start_year
  FROM employee_vw;
  ```

**Table Links**: select from more than one table

```mysql
SELECT employee.emp_id, employee.fname,
  employee.lname, department.name dept_name
FROM employee INNER JOIN department
  ON employee.dept_id = department.dept_id;
  
# with aliases
SELECT e.emp_id, e.fname, e.lname, d.name dept_name 
FROM employee (AS) e INNER JOIN department (AS) d
  ON e.dept_id = d.dept_id;
```

### The WHERE Clause

Filters out unwanted data

```mysql
SELECT emp_id, fname, lname, start_date, title
FROM employee
WHERE (title = 'Head Teller' AND start_date > '2006-01-01')
  OR (title = 'Teller' AND start_date > '2007-01-01');
```

### The ORDER BY Clause

```mysql
SELECT open_emp_id, product_cd
FROM account
ORDER BY open_emp_id, product_cd;

# Descending sort order
SELECT account_id, product_cd, open_date, avail_balance
FROM account
ORDER BY avail_balance DESC;

# Sorting via expressions
SELECT cust_id, cust_type_cd, city, state, fed_id
FROM customer
ORDER BY RIGHT(fed_id, 3);
```

## Filtering

A **condition** is made up of one or more **expressions** coupled with one or more **operators**

- **Expressions**:
  - A number
  - A column in a table or view
  - A string literal, such as 'Teller'
  - A built-in function, such as concat('Learning', ' ', 'SQL')
  - A subquery
  - A list of expressions, such as ('Teller', 'Head Teller', 'Operations Manager')
- **Operators**:
  - Comparison operators, such as =, !=, <, >, <>, LIKE, IN, and BETWEEN
  - Arithmetic operators, such as +, −, *, and /

### Condition Types

```mysql
# Equality conditions
# 'column = expression'
SELECT pt.name product_type, p.name product
FROM product p INNER JOIN product_type pt
  ON p.product_type_cd = pt.product_type_cd
WHERE pt.name = 'Customer Accounts';

# Inequality conditions
SELECT pt.name product_type, p.name product
FROM product p INNER JOIN product_type pt
  ON p.product_type_cd = pt.product_type_cd
WHERE pt.name <> 'Customer Accounts';

# Data modification using equality conditions
DELETE FROM account 
WHERE status = 'CLOSED' AND YEAR(close_date) = 2002;

# Range conditions
SELECT emp_id, fname, lname, start_date
FROM employee
WHERE start_date < '2007-01-01';
# Using BETWEEN operator
SELECT emp_id, fname, lname, start_date
FROM employee
WHERE start_date BETWEEN '2005-01-01' AND '2007-01-01';

# Membership conditions
SELECT account_id, product_cd, cust_id, avail_balance 
FROM account 
WHERE product_cd IN ('CHK','SAV','CD','MM');
# Using subqueries
SELECT account_id, product_cd, cust_id, avail_balance
FROM account
WHERE product_cd IN (SELECT product_cd FROM product
  WHERE product_type_cd = 'ACCOUNT');
  
# Matching conditions
SELECT emp_id, fname, lname
FROM employee
WHERE LEFT(lname, 1) = 'T';
# using wildcards
# _: exactly one character
# %: any number of characters (including 0)
SELECT lname
FROM employee
WHERE lname LIKE 'F%';
# using regular expressions
SELECT emp_id, fname, lname
FROM employee
WHERE lname REGEXP '^[FG]';

# is null
SELECT emp_id, fname, lname, superior_emp_id
FROM employee
WHERE superior_emp_id IS NULL;
```

## Set Operators

```mysql
# UNION (without duplicates)
# UNION ALL (with duplicates)
SELECT 'IND' type_cd, cust_id, lname name
FROM individual
UNION ALL
SELECT 'BUS' type_cd, cust_id, name
FROM business;

# INTERSECT
SELECT emp_id 
FROM employee 
WHERE assigned_branch_id = 2 
  AND (title = 'Teller' OR title = 'Head Teller')
INTERSECT 
SELECT DISTINCT open_emp_id 
FROM account 
WHERE open_branch_id = 2;

# EXCEPT
SELECT emp_id 
FROM employee 
WHERE assigned_branch_id = 2 
  AND (title = 'Teller' OR title = 'Head Teller')
EXCEPT 
SELECT DISTINCT open_emp_id 
FROM account 
WHERE open_branch_id = 2;
```

## Subqueries

### Noncorrelated Subqueries

```mysql
# A single row with a single column
# using the usual operators (=, <>, <, >, <=, >=)
SELECT account_id, product_cd, cust_id, avail_balance
FROM account
WHERE account_id = (SELECT MAX(account_id) FROM account);

# Multiple rows with a single column
# use IN, NOT IN
SELECT emp_id, fname, lname, title
FROM employee
WHERE emp_id IN (SELECT superior_emp_id
   FROM employee);
# use ALL, ANY
SELECT account_id, cust_id, product_cd, avail_balance
FROM account
WHERE avail_balance < ALL (SELECT a.avail_balance
  FROM account a INNER JOIN individual i
  ON a.cust_id = i.cust_id
  WHERE i.fname = 'Frank' AND i.lname = 'Tucker');
  
SELECT account_id, cust_id, product_cd, avail_balance
FROM account
WHERE avail_balance < ANY (SELECT a.avail_balance
  FROM account a INNER JOIN individual i
  ON a.cust_id = i.cust_id
  WHERE i.fname = 'Frank' AND i.lname = 'Tucker');
  
# Multiple rows and columns
SELECT account_id, product_cd, cust_id
FROM account
WHERE (open_branch_id, open_emp_id) IN
  (SELECT b.branch_id, e.emp_id
   FROM branch b INNER JOIN employee e
     ON b.branch_id = e.assigned_branch_id
   WHERE b.name = 'Woburn Branch'
     AND (e.title = 'Teller' OR e.title = 'Head Teller'));
```

### Correlated Subqueries

```mysql
# count the number of accounts for each customer, and the containing query then retrieves those customers having exactly two accounts
SELECT c.cust_id, c.cust_type_cd, c.city
FROM customer c
WHERE 2 = (SELECT COUNT(*)
  FROM account a
  WHERE a.cust_id = c.cust_id);
```

#### The EXISTS Operator

```mysql
# finds all the accounts for which a transaction was posted on a particular day
SELECT a.account_id, a.product_cd, a.cust_id, a.avail_balance 
FROM account a 
WHERE EXISTS (SELECT 1
  FROM transaction t 
  WHERE t.account_id = a.account_id 
    AND t.txn_date = '2008-09-22');
```

### When to Use Subqueries

- as data sources
- in filter conditions
- as expression generators


## Conditional Logic

```mysql
# searched case expression
CASE 
  WHEN C1 THEN E1
  WHEN C2 THEN E2
  ...
  WHEN CN THEN EN
  [ELSE ED]
END

CASE
  WHEN employee.title = 'Head Teller'
    THEN 'Head Teller'
  WHEN employee.title = 'Teller'
    AND YEAR(employee.start_date) > 2007
    THEN 'Teller Trainee'
  WHEN employee.title = 'Teller'
    AND YEAR(employee.start_date) < 2006
    THEN 'Experienced Teller'
  WHEN employee.title = 'Teller'
    THEN 'Teller'
  ELSE 'Non-Teller'
END

# simple case expression
CASE V0
  WHEN V1 THEN E1
  WHEN V2 THEN E2
  ...
  WHEN VN THEN EN
  [ELSE ED]
END

CASE customer.cust_type_cd
  WHEN 'I' THEN
   (SELECT CONCAT(i.fname, ' ', i.lname)
    FROM individual I
    WHERE i.cust_id = customer.cust_id)
  WHEN 'B' THEN
   (SELECT b.name
    FROM business b
    WHERE b.cust_id = customer.cust_id)
  ELSE 'Unknown Customer Type'
END
```



## Transactions

### Locking

- request write lock to write data, read requests are *blocked* until the write lock is released
- request write lock to write data, read requests do not need any lock to query data

#### Lock Granularities

- Table Lock
- Page Lock
- Row Lock

### Transaction

- MySQL: auto-commit, some storage engine support transaction
- deadlock: when two different transactions are waiting for resources that the other transaction currently holds

```mysql
# disable auto-commit mode
SET AUTOCOMMIT=0

[START TRANSACTION] # unnecessary for mysql
SAVEPOINT my_savepoint;
[some updates]
ROLLBACK TO SAVEPOINT my_savepoint;
[some updates]
COMMIT;
```



## Indexed and Constraints

### Index

- Balanced tree indexes
- Bitmap indexes
- Text indexes

```mysql
ALTER TABLE department
ADD UNIQUE dept_name_idx (name);

# Multicolumn indexes
ALTER TABLE employee
ADD INDEX emp_names_idx (lname, fname);
```

### Constraint

- *Primary key constraints*: Identify the column or columns that guarantee uniqueness within a table
- *Foreign key constraints*: Restrict one or more columns to contain only values found in another table’s primary key columns
- *Unique constraints*: Restrict one or more columns to contain unique values within a table
- *Check constraints*: Restrict the allowable values for a column

```mysql
# create table with primary and foreign keys
CREATE TABLE product
 (product_cd VARCHAR(10) NOT NULL,
  name VARCHAR(50) NOT NULL,
  product_type_cd VARCHAR (10) NOT NULL,
  date_offered DATE,
  date_retired DATE,
    CONSTRAINT fk_product_type_cd FOREIGN KEY (product_type_cd)
      REFERENCES product_type (product_type_cd),
    CONSTRAINT pk_product PRIMARY KEY (product_cd)
);

# add the primary and foreign key constraints
ALTER TABLE product
ADD CONSTRAINT pk_product PRIMARY KEY (product_cd);

ALTER TABLE product
ADD CONSTRAINT fk_product_type_cd FOREIGN KEY (product_type_cd)
 REFERENCES product_type (product_type_cd);

# drop the primary and foreign key constraints
ALTER TABLE product
DROP PRIMARY KEY;

ALTER TABLE product
DROP FOREIGN KEY fk_product_type_cd;
```

#### Cascading Constraints

propagate update/delete to child table

```mysql
ALTER TABLE product
ADD CONSTRAINT fk_product_type_cd FOREIGN KEY (product_type_cd)
 REFERENCES product_type (product_type_cd)
 ON UPDATE CASCADE
 ON DELETE CASCADE;
```



# MondoDB

Import

```python
from pymongo import MongoClient
```

Initiation
```python
client = MongoClient()
client = MongoClient('localhost', 27017)
# creat or access database
db = client.test_db
db = client['test_db']
# creat or access collection
coll = db.test_collection
coll = db['test_collection']
```

Insert and Query
```python
# insert
coll.insert_one({'test': 'hello world', 'test2': 123})
coll.insert_many([{'Index': 1, 'Content': 'a'}
    {'Index': 2, 'Content': 'b'}
    {'Index': 3, 'Content': 'c'}
    ])
# find one
coll.find_one({'Content': 'b'})
# find all
coll.find()
coll.find({'Content': 'b'})
# find by id
id = coll.insert_one({'test': 'hello world'}).inserted_id
coll.find_one({"_id": id})
```

Modify
```python
# update with $set
coll.update_one(
	{'test': 'hello world'}, 
	{
		'$set':{
			'update1': 'u1'
			'update2': 'u2'
		}
	}
)
# replace
coll.replace_one(
	{'test': 'hello world'}, 
	{
		'test': 'goodbye world',
		'test2': '456'
	}
)
```

Information
```python
# print all collections
db.collection_names(include_system_collections=False)
# counts
coll.count()
```
Export to pandas

```python
df = pd.DataFrame(list(collection.find()))
```

